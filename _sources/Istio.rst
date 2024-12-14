=====
Istio
=====

1. `What is Istio?`_
2. `Basics`_
3. `Traffic Management`_
4. `Security`_
5. `Observability`_

What is Istio?
==============


Service Mesh
------------
    * infrastructure layer for handling communication between services without changing the
    code in microservice architecture
        - replace all requirements, other than business logic, with proxy as sidecar container
        - proxies communicate with each other through data plane and communicate to server
          side component, control plane, which manages all traffic into and out of services via
          proxies
    * can dynamically change how services communicate
    * **Control Plane**
        - citadel: manage certificate generation
        - pilot: service discovery
        - galley: validate configuration files
* Istio is a free and open-source service mesh
* works with Kubernetes and traditional workloads
* provides universal traffic management, telemetry and security
* Istio implements proxies using Envoy, an open-source proxy

Istiod
------
    * daemon of combination of citadel, pilot and galley of control plane

Istio Agent
-----------
    * Envoy proxy responsible for passing configuration secrets to other proxies

Installation
------------
    * **Istioctl**
        - download from https://istio.io/latest/docs/setup/getting-started/#download  and run
          ``istioctl install --set profile=demo -y``, other profiles also available
        - Istio will be deployed as deployment in the cluster as istiod, in the namespace
          istio-system
        - also deploys istio-ingressgateway and istio-egressgateway services, and other
          necessary service objects
        - verify installation with ``istioctl verify-install``
    * **Istio Operator**
    * **Using Helm**

`back to top <#istio>`_

Basics
======

* ``istioctl install``: install Istio on cluster
* ``istioctl verify-install``: verify Istio installation
* ``istioctl analyze``: analyze and validate configuration
* need to explicitly enable Istio sidecar injection at namespace level for applications to have
proxy services
    * ``kubectl label namespace default istio-injection=enabled``

`back to top <#istio>`_

Traffic Management
==================


Istio Gateways
--------------
    * load balancers that sit on the edge of the mesh
    * main configuration that manage inbound, by istio-ingressgateway, and outbound, by
    istio-egressgateway traffic to service mesh
    * recommended approach other than using Kubernetes ingress
    * ingress and egress gateways are standalone Envoy proxies, not sidecars
    * can also have custom gateway controllers

Virtual Services
----------------
    * define a set of routing rules for traffic coming from ingress gateway
    * can specify traffic behaviour for one or more hostname
    * can manage traffic within different versions of a service
    * support standard and regex pass
    * when virtual service is created, Istio control plane apply the new configuration to all
    Envoy sidecar proxies
    * can configure for A/B testing

Destination Rules
-----------------
    * apply router policies after traffic is routed to a service
    * Envoy load balances traffic in round-robin by default, and it can be customized through
    destination rules by specifying traffic policy of load balancer, such as passthrough,
    round-robin, least conn, random
    * when using short domain names, Istio will interpret the short name based on the rules
    namespace, not the services actual name space
    * using fully qualified domain names is recommended

Fault Injection
---------------
    * testing approach to check if error handling mechanisms are working
    * can verify if policies run efficiently
    * can inject errors in virtual services, delays and aborts

Timeouts
--------
    * if a service is taking too much time to respond, it must not keep the dependent service
    waiting forever, but must fail after a period of time and return error message
    * can be tested with fault injection
    * can use timeouts when services are dependent on each other and to make clear definitions
    between the dependencies
    * specified on virtual service level

Retries
-------
    * configuring virtual service to attempt request again if a service cannot be reached
    * no need to handle the retries in the application code
    * Istio by default has 25ms+ intervals after first fail and 2 retires before returning an
    error
    * specified on virtual service level

Circuit Breaking
----------------
    * marking the requests as failed immediately when a service breaks or slows down
    * to prevent delay of dependent service from waiting of the down one
    * allow to create resilient microservice application that limit the impact of failures or
    network-related issues
    * can also be used to limit number of requests coming into a service
    * configured in destination rules

`back to top <#istio>`_

Security
========

* traffic between services need to be encrypted and some services need to implement access
control restrictions
* Istio support mutual TLS, access policies and audit logs
* more details of [Istio Security Architecture](https://istio.io/latest/docs/concepts/security/)
* API server configuration distributes all authentication, authorization and secure naming
policies to the proxies
* sidecar and ingress/egress proxies work as Policy Enforcement Points and certificate, keys,
authentication, authorization and secure naming policies are sent to them at all times
* security at depth: every point having security checks, not just certain entry point to the
network

Authentication
--------------
    * traffic between peers/services must be hardened by using verification options such as
    mTLS and JSON web tokens
    * with mTLS, each service gets its own identity, using certificate key pairs
    * certificates are generated and distributed automatically by istiod
    * end users' access to services can be authenticated using JSON web token or OpenID Connect
    providers
    * can have policies of workload-specific, namespace-wide or mesh-wide, by setting namespace
    to istio-system

Authorization
-------------
    * to access control for inbound traffic
    * can control which service can reach which service, east-west traffic using authorization
    configuration
    * can define policies to allow or deny requests based on criteria
    * implemented by Envoy proxies authorization engine in runtime
    * when a request comes to a proxy, the engine evaluate the request context against current
    authorization polices and returns the result
    * supported actions: allow, deny, custom (allows an extension to handle the request)
    * authorization policies can also be configured to audit requests

Certificate Management
----------------------
    * a service needs to identify itself to the mesh control plane when it is started and
    retrieve a certificate to serve traffic
    * istio agent creates a private key and certificate signing request and send it to istiod
    * certificate authority in istiod validates the credentials, signs the CSR and generate a
    certificate
    * istio agent then sends the certificate and private key to envoy
    * istio agent monitor the expiration of the certificate
    * production clusters should use production-ready CA such as HashiCorp Vault

`back to top <#istio>`_

Observability
=============


Kiali
-----
    * to visualize, manage, define and validate connections and microservices of service mesh
    * has a web-based GUI and provides visibility for features such as request routing, circuit
    breakers, request rates and latency
    * provides wizards to apply common traffic patterns and can auto generate Istio configuration
    * ``kubectl -n istio-system get svc kiali``: check if Kiali is deployed
    * ``istioctl dashboard kiali``: open Kiali dashboard, default port 20001

Monitoring
----------
    * standard Istio metrics are exported to Prometheus by default and can be visualized with
    Grafana
    * ``istioctl dashboard prometheus``: open Prometheus dashboard, default port 9090
    * ``istioctl dashboard grafana``: open Grafana dashboard, default port 3000

Distributed Tracing
-------------------
    * monitoring individual requests as a flow through a mesh
    * service dependencies and sources of latency within the service mesh can be observed by
    tracing
    * Istio enables distributed tracing through Envoy proxies and support Zipkin, Jaeger,
    Lightstep and DataDog
    * ``istioctl dashboard jaeger``: open Jaeger dashboard, default port 16686

`back to top <#istio>`_
