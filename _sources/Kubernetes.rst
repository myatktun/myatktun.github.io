==========
Kubernetes
==========

1. `What is K8s?`_
2. `Basics`_
3. `Architecture`_
4. `Setting up K8s`_
5. `kubectl`_
6. `etcd`_
7. `Pods`_
8. `Services`_
9. `Workloads`_
10. `Storage & Volumes`_
11. `Controller`_
12. `Scheduler`_
13. `Networking`_
14. `Security`_
15. `Configuration`_
16. `Observability`_
17. `Cluster Maintenance`_
18. `Microservices Architecture`_
19. `Designing a Cluster`_
20. `System Hardening`_
21. `Tools`_
22. `Exam Guide`_
23. `Additional Resources`_

What is K8s?
============

* developed by Google and became open source
* orchestrate containers and configure auto scaling as needed
* have more options and features than other orchestration tools
* can upgrade instances with rolling upgrade fashion one at a time

    .. code-block:: sh

       kubectl rolling-update my-app --image=app:2


* can roll back if error occurs

    .. code-block:: sh

       kubectl rolling-update my-app --rollback


* can test new features by only upgrading a few of instances through A/B testing
* open architecture provides support for many network and storage vendors
* supports a variety of authentication and authorization mechanisms

Principles
----------
    * Kube API declarative over imperative
        - self-healing, able to rollback, extensible
    * No hidden internal APIs
        - immutable regardless of the environment
    * Meet the user where they are
        - ease of adoption
    * Workload portability
        - cloud/cluster agnostic, separation of concerns
* K8s achieves scalability by favoring **decoupled architectures**
    * each component is separated from other components by defined APIs and service load
    balancers
    * APIs provide buffer between implementer and consumer
    * load balancers provide buffer between running instances of each service
    * K8s provide numerous abstractions and APIs to make building decoupled microservice
    architectures easier

`back to top <#kubernetes>`_

Basics
======

* `Velocity`_, `Project Maturity`_, `YAML`_, `TLS Basics`_, `Switching & Routing`_, `DNS`_, `Network Namespace`_
* `Container Network Configuration`_, `CRI`_, `CSI`_, `CNI`_, `Container Sandboxing`_
* `System Calls`_, `AppArmor`_

Velocity
--------
    * key component in nearly all software development
    * measured not in terms of raw number of features shipped per hour or day, but rather in
    terms of number of things shipped while maintaining high availability service

Project Maturity
----------------
    * **Sandbox**
        - project is still in early development and adoption is not recommended
        - only early adopter or those interested in contribution should adopt the project
    * **Incubator**
        - projects that have proven utility and stability via adoption and production usage
        - but still developing and growing communities
    * **Graduated**
        - full mature and widely adopted

YAML
----
    * dictionary (key: value), unordered
        .. code-block:: yaml

           # comment
   
           color: blue
           type: car
           model:
               name: Model
               year: 1999

    * list (multiple items of same type), ordered
        .. code-block:: yaml

           - car1 blue
           - car2 red
           - car3 white

    * list of dictionaries
        .. code-block:: yaml

           - color: blue
             type: car
             model:
               name: Model
               year: 1999
   
           - color: blue
             type: car
             model:
               name: Model
               year: 1999
   
           - color: blue
             type: car
             model:
               name: Model
               year: 1999

    * K8s YAML contains four top definition fields
        - **apiVersion**
          + K8s api used to create objects
        - **kind**
          + type of object to create
          + e.g Pod (v1), ReplicaSet (apps/v1), Service (v1), Deployment (apps/v1)
        - **metadata**
          + data about object such as name, labels
          + every data is in the form of dictionary
          + indent doesn't matter as long as they are same
          + labels can have any key/value pairs and make pods easier to group
        - **spec**
          + remaining details such as containers, which contains arrays

TLS Basics
----------
    * used to guarantee trust during transactions
    * symmetric encryption
        - using same key to encrypt and decrypt, key has to be exchanged between receiver and
          sender
        - data is encrypted using a key
        - a copy of the key must also be on server
        - e.g data transaction between user and web server
    * asymmetric encryption
        - uses a pair of keys, private and public
        - data encrypted with public key, can only be decrypted with it's private key
        - public key can be shared
        - cannot encrypt and decrypt with the same key
        - use one to decrypt and the other to decrypt
        - if data is encrypted using private key, public key can be used to decrypt and since
          public key has been shared, deciding which key to use to decrypt is important
        - e.g safely transfer symmetric key between user and web server
    * ``ssh-keygen`` is for ssh purposes, format is different
    * use ``openssl`` for web servers
        - ``openssl genrsa -out my.key 1024``
        - ``openssl rsa -in my.key -pubout > myPub.pem``
        - when the user first access the web server using https, he gets the public key from
          the server, hackers can get the public key
        - the user's browser encrypt the symmetric key using the public key provided by the
          server
        - the user sends the encrypted key to the server, hacker also gets the copy
        - the server decrypt the symmetric key using it's private key, since the hacker does
          not have the private key, he cannot decrypt it
        - the symmetric key now be used to send data between user and server, the server can
          use the same encrypted symmetric key to decrypt the data
    * every certificate has a name, person or subject the certificate is issued to
    * who signed and issued the certificate is very important
    * everyone can look at the certificate and know if the persons signed is authorized or not
    * all the browsers have built-in certificate validation
    * Certificate Authority (CA)
        - well-known organizations that can sign and validate the certificate
        - e.g Symantec, Digicert, Comodo, GlobalSign
        - generate a CSR (Certificate Signing Request) using the key generated and the domain
          name of the website
        - ``openssl req -new -key my.key -out my.csr -subj "/C=US/ST=CA/O=MyOrg, Inc./CN=mydomain.com``
        - CA verifies the details and sign the certificate and send it back
        - now have a certificate that the browsers trust
        - CAs make sure that the sender is the actual owner of the domain
        - CAs use their private key to sign the certificate
        - their public keys are built-in to the browser
        - the browser uses the CAs public key to validate the certificate
        - public CAs don't help to validate sites hosted privately
        - can host private CAs within organization and use the keys
    * client certificates
        - the server does not know if the client can be trusted or not
        - in initial trust building exercise, the server can request a certificate from the
          client
        - the client generates a pair of keys and a signed certificate from a CA and send it
          to the server
        - TLS client certificates are not generally implemented on web servers
    * certificates with public key are usually named ``*.crt`` or ``*.pem``
    * private keys are usually named ``*.key`` or ``*-key.pem``
    * PKI (Public Key Infrastructure)
        - CAs, servers, clients and the process of generating, distributing and maintaining
          digital certificates
    * **Oneway SSL**
        - client verifies server's certificate, the server does not verify client's certificate
        - the server only verifies the user based on the data sent, such as username, password
    * **Mutual SSL**
        - both client and server verify authenticity of each other
        - client first request server's public certificate
        - the server replies with it's public certificate and requests for the client's public
          certificate
        - the client checks the server's certificate with the CA
        - the client then sends its public certificate and a symmetric key encrypted with the
          public key of the server
        - the server verifies the client's certificate with the CA
        - all communication can now be encrypted with symmetric keys

Switching & Routing
-------------------
    * to connect computers/systems to a switch, an interface is needed on each host, physical
    or virtual
    * once the IPs are assigned to the interfaces, the systems can now communicate through the
    switch
    * the switch can only enable communication within the same network
    * for systems to reach other networks, a router is needed
    * a router connects separate networks, getting assigned IPs on each network
    * a gateway in the network is required for a system to send packets to through it
    * connect the router to the Internet and add a new route, so that the systems can reach the
    Internet
    * for any network the system does not know a route to, can let it use the router as default
    gateway
    * 0.0.0.0 entry in Gateway means the system does not need a gateway to reach the
    destination
    * if there are multiple routers for different networks, separate entries for each network are
    required
    * by default in Linux, packets are not forwarded from one interface to the next
    * to allow packet forwarding, edit the ``/pro/sys/net/ipv4/ip_forward`` (0 to disable and 1
    to enable), configuration does not persist through reboots
    * to persist the configuration, set the ``net.ipv4.ip_forward`` value in ``/etc/sysctl.conf``

    .. code-block:: sh

       # check interface
       ip link
   
       # assign IP to the interface
       ip addr add 192.168.1.10/24 dev eth0
   
       # check routing tables
       route
   
       # add gateway (the system can reach 192.168.2.0 through 192.168.1.1)
       ip route add 192.168.2.0/24 via 192.168.1.1
   
       # use router as default gateway
       ip route add default via 192.168.1.1



DNS
---
    * allow systems to communicate using names instead of IP addresses
    * **Name Resolution**
        - translating IP addresses to names
        - edit ``/etc/hosts`` with IP address and name
        - hostname on other system doesn't matter, the system uses what's defined in ``/etc/hosts``
    * **DNS server**
        - as the environment grows, entries in ``/etc/hosts`` will also increase
        - instead of managing entries on each system, let a single server, DNS server, manage
          all entries
        - point all systems to look up on the DNS server if they need to resolve a name to IP
          address
        - every system has DNS resolution configuration file at ``/etc/resolv.conf``
        - only need to update on the DNS server if IPs change
        - if there is same entry on local and DNS server, the system will first look in local
          file and use it
        - order that defines which file to look first is defined in ``/etc/nsswitch.conf``
        - can configure the DNS server to forward all unknown names to the public nameserver
    * **Domain Names**
        - root: .
        - top-level domains: .com, .net, .edu, .org, .io
        - domain name: google
        - subdomain: www, drive, mail
        - a server can cache the IP for faster resolves
    * **Record Types**
        - A: storing IPs
        - AAAA: storing IPv6
        - CNAME: mapping one name to another name
    * can use tools such as ping, nslookup, dig
    * **CoreDNS**
        - DNS server solution, listens on port 53 by default
        - put entries in ``/etc/hosts``, configure CoreDNS to use the file
        - CoreDNS loads configurations from a file named Corefile

Network Namespace
-----------------
    * used by containers for network isolation
    * can connect namespaces using virtual ethernet pairs, pipes

    .. code-block:: sh

       # list network namespaces
       ip netns
   
       # create network namespace
       ip netns add namespace1
       ip netns add namespace2
   
       # view interfaces on the namespace
       ip netns exec namespace1 ip link
       ip -n red link
   
       # connect namespace
       ip link add veth-namespace1 type veth peer name veth-namespace2
   
       # attach interface to appropriate namespace
       ip link set veth-namespace1 netns namespace1
       ip link set veth-namespace2 netns namespace2
   
       # assign IP in namespace
       ip -n namespace1 addr add 192.168.15.1 dev veth-namespace1
       ip -n namespace2 addr add 192.168.15.2 dev veth-namespace2
   
       # bring up interface
       ip -n namespace1 link set veth-namespace1 up
       ip -n namespace2 link set veth-namespace2 up


    * create a virtual switch to connect multiple namespaces using solutions such as Linux
    Bridge, Open vSwitch

        .. code-block:: sh

           # add new interface
           ip link add v-net-0 type bridge
   
           # bring up interface
           ip link set dev v-net-0 up
   
           # delete existing direct link, other end is auto deleted
           ip -n namespace1 link del veth-namespace1
   
           # connect namespace to the bridge
           ip link add veth-namespace1 type veth peer name veth-namespace1-br
           ip link add veth-namespace2 type veth peer name veth-namespace2-br
           ip link set veth-namespace1 netns namespace1
           ip link set veth-namespace1-br master v-net-0
           ip link set veth-namespace2 netns namespace2
           ip link set veth-namespace2-br master v-net-0
   
           # assign IP
           ip -n namespace1 addr add 192.168.15.1 dev veth-namespace1
           ip -n namespace2 addr add 192.168.15.2 dev veth-namespace2
   
           # bring up interface
           ip -n namespace1 link set veth-namespace1 up
           ip -n namespace2 link set veth-namespace2 up


    * can use bridge to connect host and namespaces by assigning IP to the bridge interface

        .. code-block:: sh

           ip addr add 192.168.15.5/24 dev v-net-0


    * for namespaces to reach a network through the host port, the host can be used as a
    gateway
        - the host now have two IPs, one on bridge and other on external
        - gateway should be the one the namespace can reach, the bridge (192.168.15.5)
        - the host must have net functionality for the outside network to know that the
          packets are coming from the host instead of the namespace, which is unknown it
        - since the namespace can reach any network the host can, it can be setup so that
          the namespace talk to the host to reach any external network
        - can use port forwarding for external network to reach the namespace

        .. code-block:: sh

           # use host as gateway to reach 192.168.1.0
           ip netns exec namespace1 ip route add 192.168.1.0/24 via 192.168.15.5
   
           # enable net function
           iptables -t nat -A POSTROUTING -s 192.168.15.0/24 -j MASQUERADE
   
           # use host as default gateway to reach any network
           ip netns exec namespace1 ip route add default via 192.168.15.5
   
           # forward packets coming to host's port 80 to the namespace
           iptables -t nat -A PREROUTING --dport 80 --to-destination 192.168.15.1:80 -j DNAT



Container Network Configurations
--------------------------------
    * **none**
        - container is not part of any network
    * **host**
        - container is attached to host network, no network isolation between host and
          container
    * **bridge**
        - internal private network is created, which the host and containers attach to and
          containers get their own internal private IPs
        - docker calls the default bridge as 'bridge', but the host sees it as 'docker0'
        - docker creates a network namespace for a container by default and attach it to the
          bridge, naming interfaces as odd/even pairs
        - docker provides port mapping for external networks to reach the container

Container Runtime Interface (CRI)
---------------------------------
    * to communicate with container runtime such as docker, rkt, cri-o

Container Storage Interface (CSI)
---------------------------------
    * can have custom driver to work with custom storage on K8s such as PortWorx, AWS EBS, EMC
    * not just a K8s standard, universal one to allow any container orchestration tool to work
    with storage vendors, K8s, Cloud Foundry and Mesos are on board
    * CSI defines sets of RPC

Container Network Interface (CNI)
---------------------------------
    * set of standards to extend support for different networking solutions
    * a plugin is built using the standards
    * different container runtimes use the plugin by passing cid and namespace to configure
    network for the container
    * **standards for container runtimes**
        - container runtime must create network namespace
        - identify network the container must attach to
        - container runtime to invoke Network Plugin when container is added
        - container runtime to invoke Network Plugin when container is deleted
        - JSON format of the Network Configuration
    * **standards for plugins**
        - must support command line arguments ADD/DEL/CHECK
        - must support parameters such as container id, network ns
        - must manage IP address assignment to pods
        - must return results in a specific format
    * support bridge, vlan, ipvlan, macvlan, windows, dhcp, host-local
    * third-party plugins such as Weave-net, Flannel, Cilium, VMware NSX
    * **Container Network Model (CNM)**
        - docker's own standards, does not use CNI but similar
        - cannot use ``docker run --network=cni-bridge nginx``
        - must create container without network and manually invoke the plugin

Container Sandboxing
--------------------
    * using Seccomp, AppArmor
    * allowlist
        - blocks everything by default
        - useful when every syscalls of the application is known
    * denylist
        - allows everything by default
        - useful when want to make rules not much restricted or want to apply the rules to
          different applications
    * writing efficient profiles for hundreds of application is not easy, even with third-party
    tools
    * choose which best work for the environment

System Calls
------------
    * applications and processes, in user space, make system calls to kernel, in kernel space,
    to perform anything
    * by default, Linux allows processes to invoke any syscalls from user space
    * **strace**
        - ``strace touch /tmp/newfile.log``, ``strace -c touch /tmp/newfile.log``
        - ``strace -p PID``, trace a running process
    * **Aquasec tracee**
        - make use of eBPF, which can run programs directly in kernel space without
          interfering with kernel code or loading modules
        - need bind mounts when using as container, ``/tmp/tracee``, ``/lib/modules``, ``/usr/src``
        - container also needs to privileged
        - ``trace comm=ls``, ``trace pid=new``, ``trace container=new``
    * **seccomp**
        - Secure Computing, kernel level feature to sandbox processes to only use necessary
          syscalls
        - ``grep Seccomp /proc/pid/status``, Seccomp value 2 means it is implemented
        - modes: 0 (disabled), 1 (strict), 2 (filtered)
        - docker has built-in seccomp filter used by default, if the host kernel has seccomp
        enabled
        - using allowlist type will need to know every syscalls made by the application
        - denylist type are easier and flexible, but susceptible to attacks
        - container with custom seccomp, ``docker run --security-opt seccomp=/root/custom.json``
        - docker's default seccomp profile blocks 64 syscalls and mode 2
        - cannot restrict a program access to certain objects, such as files or directory

        .. code-block:: json

           {
               "defaultAction": "SCMP_ACT_ERRNO",
               "architectures": [
                   "SCMP_ARCH_X86_64",
                   "SCMP_ARCH_X86",
                   "SCMP_ARCH_X32"
               ],
               "syscalls": [
                   {
                       "names": [
                           "syscall-1",
                           "syscall-2",
                           "syscall-3"
                       ],
                       "action": "SCMP_ACT_ALLOW"
                   }
               ]
           }


        .. code-block:: json

   
           {
               "defaultAction": "SCMP_ACT_ALLOW",
               "architectures": [
                   "SCMP_ARCH_X86_64",
                   "SCMP_ARCH_X86",
                   "SCMP_ARCH_X32"
               ],
               "syscalls": [
                   {
                       "names": [
                           "syscall-1",
                           "syscall-2",
                           "syscall-3"
                       ],
                       "action": "SCMP_ACT_ERRNO"
                   }
               ]
           }



AppArmor
--------
    * Linux security module to confined a program to limited resources
    * installed by default on Debian/Ubuntu, ``cat /sys/module/apparmor/parameters/enabled``
    * check loaded profiles, ``cat /sys/kernel/security/apparmor/profiles``
    * [profiles](profiles) are text files that define what resources can be used by application
    * check profiles loaded, ``aa-status``
    * modes: enforce, complain, unconfined
    * use ``apparmor-utils`` to create profiles
        - ``aa-genprof /path/to/app``, need to run the app so that it can scan events
        - severity range with 10 being highest
        - ``cat /etc/apparmor.d/root.app``, default profile location
        - ``apparmor_parser /etc/apparmor.d/root.app``, load profile
        - disable profile, ``apparmor_parser -R /etc/apparmor.d/root.ap``, and
          ``ln -s /etc/apparmor.d/root.app /etc/apparmor.d/disable/``

    .. code-block:: text

       # apparmor-deny-write
       profile apparmor-deny-write flags=(attach_disconnected) {
           file, # allow access to entire filesystem
           deny /** w, # deny all file writes
       }
   
       # apparmor-deny-remount-root
       profile apparmor-deny-remount-root flags=(attach_disconnected) {
           deny mount options=(ro, remount) -> /, # deny remount readonly the root filesystem
       }


`back to top <#kubernetes>`_

Architecture
============

* `Nodes`_, `Cluster`_, `Control Plane`_, `Worker Nodes`_
* many of the component that make up the cluster are actually deployed using K8s itself, which
are in `kube-system` namespace

Nodes
-----
    * physical or virtual machine on which K8s tools are installed
    * **kubelet**
        - agent that runs on each node in the cluster
        - listens for instructions form kube-apiserver
        - responsible for making sure that the containers are running on the node as expected
        - point of contact for nodes with the control-plane
        - monitor and sends back status of the nodes at regular interval
        - registers the node with K8s cluster
        - requests the CRI a pod needs to be loaded
        - kubeadm does not auto depoly kubelet
        - must always manually install on worker nodes
        - can be searched as a process ``ps -aux | grep kubelet`` on the worker node
    * **kube-proxy**
        - responsible for routing network traffic to load-balanced services in the cluster
        - process that is required to run on each node
        - ensures that necessary rules are placed on worker nodes to allow containers to
          communicate
        - looks for new services and creates appropriate rules on each node to forward traffic
          to those services to the backend pods
        - create IP table rules on each node and forward IP heading of the service to that of
          the actual pod
        - can download from K8s releases and run it as a service
        - list proxies, ``kubectl get daemonSets --namespace=kube-system kube-proxy``
    * **kubeadm** is deployed as pod in kube-system namespace on each node as daemonset

Cluster
-------
    * a group of nodes for high availability

Control Plane
-------------
    * a node where K8s control plane components are installed
    * watches over the nodes in cluster and is responsible for orchestration of containers in
      the worker nodes
    * has kube-apiserver, etcd, kube-controller-manager, kube-scheduler
    * **kube-apiserver**
        - front-end for K8s
        - users, devices and interfaces talk to it to interact with K8s cluster
    * **etcd**
        - distributed key/value store to store all data used to manage the cluster
    * **kube-scheduler**
        - responsible for distributing work or containers across multiple nodes
        - look for newly created containers and assign them to appropriate nodes
    * **kube-controller-manager**
        - brain behind orchestration
        - make decisions to bring up new containers
        - processes that monitor objects and respond
    * **coredns**
        - provides naming and discovery for the services in the cluster
        - DNS server also runs as a replicated service on the cluster,
          ``kubectl get depolyments --namespace=kube-system coredns``
        - DNS service is runs as a deployment,
          ``kubectl get services --namespace=kube-system coredns``

Worker Nodes
------------
    * worker machines where the containers will be launched
    * also called minions before
    * container runtime needs to be installed
    * **Container Runtime**
        - underlying software to run containers
        - eg. Docker, rkt, CRI-O, containerd

`back to top <#kubernetes>`_

Setting up K8s
==============

* `kubeadm`_, `MicroK8s`_, `minikube`_

kubeadm
-------
    * to bootstrap and manage production grade K8s clusters
    * setup
        - setup multiple systems or VMs, use tools such as Vagrant
        - designate one system as control-plane node and others as worker nodes
        - install container runtime and kubeadm tool on all nodes
        - initialize the control-plane node, required components will be installed
        - setup a POD network, make sure all network requirements are met
        - join the worker nodes to the control-plane
    * [kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)

MicroK8s
--------

minikube
--------
    * easiest way to start K8s on local system
    * bundles all components of K8s into an image, providing a single-node K8s cluster
    * bundle is available as iso file to download
    * provides an executable command line utility to auto download the iso and deploy in
      virtualization platform such as VirtualBox, KVM, HyperV
    * must have hyper visor and kubectl installed
    * [minikube](https://minikube.sigs.k8s.io/docs/start/)
* also has hosted solutions on GCP, AWS, Azure, IBM Cloud
    * **GKE**
        - need GCP account with billing enabled and ``gcloud tool`` installed
        - set default zone, ``gcloud config set compute/zone us-west1-a``
        - create cluster, ``gcloud container clusters create my-cluster --num-nodes-3``
        - get credentials, ``gcloud container clusters get-credentials my-cluster``
    * **AKS**
        - can use built-in Azure Cloud Shell in Azure portal or install ``az CLI tool``
        - create resource group, ``az group create --name=test --location=westus``
        - create cluster, ``az aks create --resource-group=test --name=my-cluster``
        - get credentials, ``az aks get-credentials --resource-group=test --name=my-cluster``
        - install kubectl if needed, ``az aks install-cli``
    * **EKS**
        - install ``eksctl tool``
        - create cluster, ``eksctl create cluster``

`back to top <#kubernetes>`_

kubectl
=======

* `apply`_, `proxy`_, `port-forward`_
* tool used to deploy and manage applications, called API objects, on the cluster

    .. code-block:: sh

       kubectl run --replicas=1000 my-app
   
       # view info about cluster
       kubectl cluster-info
       # verify cluster health
       kubectl get componentstatuses
   
       # list all the pods part of the cluster
       kubectl get pods
       # list all pods with more information
       kubectl get pods -o wide
       # remove headers from output
       kubectl get pods -o wide --no-headers
       # monitor the state
       kubectl get pods --watch
   
       # list all the nodes part of the cluster
       kubectl get nodes
   
       # create deployment
       kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
   
       # view deployment
       kubectl get deployments
   
       # expose deployment as a service
       kubectl expose deployment hello-node --type=LoadBalancer --port=8080
   
       # view services
       kubectl get services
   
       # get multiple objects
       kubectl get pods,deployments,services
   
       # list supported fields
       kubectl explain pods
   
       # execute commands in container
       kubectl exec -it myapp-pod -- sh
   
       # attach to running process
       kubectl attach -it myapp-pod
   
       # copy files from pod to local (tar must be installed in the container)
       kubectl cp myapp-pod:test.sh /local/dir/test.sh
   
       # view latest 10 events on all objects
       kubectl get events
   
       # get URL of the service if using minikube
       minikube service hello-minikube --url
   
       # clean up
       kubectl delete services hello-minikube
       kubectl delete deployment hello-minikube



kubectl apply
-------------
    * consider 'local config file', 'live object definition' and 'last applied config' before
    making changes
    * only modify objects that are different from the current objects
    * if objects already exist, it will exit successfully without making changes
    * create object if not exists and live object config is created within K8s with additional
    fields
    * local file is converted to json and stored as last applied config
    * for any update to the object, all three are compared to identify what changes are to be
    made on the live object
    * if a field in local is deleted, it is compared with last applied config and the field is
    removed in live config
    * last applied config is stored on the live object config itself as an annotation
    * can add ``--dry-run`` flag to view changes without modifying the object
    * can manipulate records with ``kubectl apply -f definition.yml edit-last-applied``, or
    ``set-last-applied``, ``view-last-applied``
    * ``kubectl create`` and ``kubectl replace`` do no store last applied config
    * do not mix imperative and declarative approaches
    * useful when using loops to create objects from multiple files

kubectl proxy
-------------
    * ``kubectl proxy`` will start a proxy, default on port 8001, which doesn't need credentials
    to access the cluster
    * will use the credentials from config file and forward the requests to kube-apiserver
        - ``curl http://localhost:8001 -k``
    * can proxy requests to any services, even if the services are not exposed
        - ``curl http://localhost:8001/api/v1/namespaces/default/services/nginx/proxy``

kubectl port-forward
--------------------
    * takes pod, deployment, service or replicaset as argument, HOST_PORT:PORT_ON_CLUSTER
    * ``kubectl port-forward service/nginx 28080:80``
    * any request to ``curl http://localhost:28080`` is forwarded to service on port 80 on the
    cluster
* ``kubectl proxy`` and ``kubectl port-forward`` are useful when accessing a remote cluster
* ``kubectl`` outputs plain-text format by default
    * ``-o`` flag allows to different format output
    * ``-o json``, ``-o yaml``
    * ``-o name``: only resource name
    * ``-o wide``: plain-text format with additional information

`back to top <#kubernetes>`_

etcd
====

* distributed key-value store that provides a reliable way to store data that needs to be
accessed by a distributed system or cluster of machines
* etcd service listens on port 2379 by default
* can attach clients to the service
* etcdctl: CLI tool to use to store key/value pairs, has version 2 (default) and 3, with
different commands
* must specify certificate files for etcdctl to authenticate etcd api server

.. code-block:: sh

   etcdctl set key1 value1
   etcdctl get key1
   
   # etcdctl version 2
   etcdctl backup
   etcdctl cluster-health
   etcdctl mk
   etcdctl mkdir
   etcdctl set
   
   # etcdctl version 3
   etcdctl snapshot save
   etcdctl endpoint health
   etcdctl get
   etcdctl put
   
   # set version
   export ETCDCTL_API=3


* stores information about the cluster
* every info from ``kubectl get`` command is from etcd server
* every change in cluster are updated in etcd server
* the change is complete only if it is updated in etcd server
* can be deployed differently
* manual setup
    * download etcd binary and configure etcd as a service on controlplane node
    * ``--advertise-client-urls https://${INTERNAL_IP}:2379``, URL that should be configured on
    kube-apiserver
* using kubeadm
    * deploy etcd service as a pod in kube-system namespace
    * ``kubectl exec etcd-master -n kube-system etcdctl get / --prefix -keys-only``, list all keys
* in high availability environment, set the right parameters to make sure multiple etcd
instances know about each other
    * ``--initial-cluster``, to specify different instances of etcd service

`back to top <#kubernetes>`_

Pods
====

* `Creating Pods`_, `Generating YAML`_, `Editing Pods`_, `Copying Files`_
* `Static Pods`_, `Init Containers`_, `Multi-Container pods`_
* objects, in nodes, in which containers are encapsulated as K8s does not deploy containers
directly on the worker nodes
* smallest object in K8s containing single instance of the application
* a new pod with a single instance is created as needed
* one-to-one relationship with containers
* each container in a pod runs in its own cgroup but share a number of Linux namespaces
* single pod can have multiple containers which are not same kind, as in helper containers
* helper container has one-to-one relationship with application container to access data, and
can communicate directly by referring to as `localhost` as they share same network space
* applications in different pods are isolated from each other
* when designing pods, it is necessary to know if containers will work correctly if they land
on different machines

Creating Pods
-------------
    * using ``kubectl run``
        - ``--image`` specify image from Docker hub, by default, or private registry

        .. code-block:: sh

           kubectl run nginx --image=nginx
   
           # list pods
           kubectl get pods
   
           # additional info about pods
           kubectl get pods -o wide
   
           # get info about pod
           kubectl describe pod POD_NAME
   
           # get logs of running instance
           kubectl logs mypod
           # get logs from previous instance of the container
           kubectl logs mypod --previous
   
           # deleting a pod can take time as it has to send killsig to the process running
           kubectl delete pod POD_NAME
           kubectl delete pods -l LABEL_NAME=LABEL


    * using manifest (YAML) file, declarative configuration
        - can use YAML or JSON
        - YAML is more preferred as it is more human-editable and supports comments

        .. code-block:: yaml

           # pod-definition.yml
           apiVersion: v1
           kind: Pod
           metadata:
             name: myapp-pod
             labels:
               app: myapp
               type: front-end
           spec:
             containers:
             - name: nginx-container
               image: nginx
               ports:
               - containerPort: 80


        .. code-block:: sh

           # create pod from yml file, 'create' and 'apply' works same if creating new pod
           kubectl create -f pod-definition.yml
           kubectl apply -f pod-definition.yml


* K8s API server accepts and processes pod manifests before storing them in etcd
* the scheduler also uses K8s API to find pods that haven't been scheduled to a node
    * scheduler tries to ensure that pods from same application are distributed onto different
    machines fro reliability
    * once scheduled, pods don't move and must be explicitly destroyed and rescheduled
* by default, all pods have a termination grace period of 30 seconds
    * it is important for reliability as it allows the pod to finish any active requests that
    it may be in the middle of processing

Generating YAML
---------------
    * ``kubectl run nginx --image=nginx --dry-run -o yaml`` will output

    .. code-block:: yaml

       apiVersion: v1
       kind: Pod
       metadata:
         creationTimestamp: null
         labels:
           run: nginx
           name: nginx
       spec:
         containers:
         - image: nginx
           name: nginx
           resources: {}
         dnsPolicy: ClusterFirst
         restartPolicy: Always
       status: {}



Editing Pods
------------
    * can only edit ``spec.containers[*].image``, ``spec.initContainers[*].image``,
    ``spec.tolerations``, ``spec.activeDeadlineSeconds``
    * cannot edit env variables, service accounts, resource limits of a running pod

    .. code-block:: sh

       # run the command and edit the file, delete and create the pod with the edited file
       kubectl get pod myapp-pod -o yaml > pod-definition.yml
   
       # to edit in text editor, the file will be saved as temp, delete the pod and use the temp
       # file tot create a new pod
       kubectl edit pod myapp-pod
       # not recommended in production
       kubectl replace --force -f /tmp/tmp-file.yml
   
       # to change image of the pod
       kubectl set image pod/myapp-pod nginx-container=nginx:NEW_VERSION
   
       # add labels
       kubectl label pods myapp-pod type=test
   
       # remove labels
       kubectl label pods myapp-pod type-



Copying Files
-------------
    * copying files into a container is an antipattern but can be an immediate way to restore
    service health

    .. code-block:: sh

       kubectl cp LOCAL_DIR mypod:POD_DIR
       kubectl cp namespace/mypod:POD_DIR LOCAL_DIR



Static Pods
-----------
    * pods that are created by kubelet on its own without the intervention of the API server or
    other K8s cluster components
    * kubelet can be configured to read the definition files from a directory on a server
    designated to store information about pods, `/etc/kubernetes/manifests`
    * kubelet will periodically read the files and create pods on the host, ensuring the pods
    stay alive
    * if the files are removed, the pods will be deleted
    * only pods can be created this way as kubelet works at pod level and only understand pods
    * can pass the directory in ``--pod-manifest`` options
    * or provide ``--config=customconfig.yml`` options and define ``staticPodPath`` in that file,
    this approach is used by cluters setup by the kubeadm
    * can view the pods with ``docker ps``, as kube-apiserver is not used
    * node name is appended at the end of the pod name, e.g etcd-controlplane, podName-node01
    * kubelet can create static pods and pods from API server at the same time, as its job is
    to create a pod that is given as input
    * if both kinds of pods are created, kube-apiserver can detect the static pods but only
    view details about the pods, cannot edit or delete them
    * if the pod is part of the cluster, kubelet also creates a mirror object of the pod in
    kube-apiserver
    * as static pods are not part of the control-plane, they can be used to deploy
    control-plane components as pods on a node

Init Containers
---------------
    * configured in a pod like all other containers
    * run when a pod is first created and the process in the container must run to a completion
    before the real container hosting the application starts
    * can configure multiple initContainers, each init container is run one at a time in
    sequential order

    .. code-block:: yaml

       # pod-definition.yml
       apiVersion: v1
       kind: Pod
       metadata:
         name: myapp-pod
         labels:
           app: myapp
           type: front-end
       spec:
         containers:
         - name: nginx-container
           image: nginx
         initContainers:
         - name: init-container1
           image: busybox
           command: ['some command to execute']
         - name: init-container2
           image: busybox
           command: ['some command to execute']



Multi-Container pods
--------------------
    * containers in the same pod share same lifecycle, network and storage
    * do not need to establish volume sharing or services between the pods to communicate
    * each container is expected to run a process that stays alive as long as the pod's
    lifecycle
    * the pod restarts if any of the containers fails

    .. code-block:: yaml

       # pod-definition.yml
       apiVersion: v1
       kind: Pod
       metadata:
         name: myapp-pod
         labels:
           app: myapp
           type: front-end
       spec:
         containers:
         - name: nginx-container
           image: nginx
         - name: log-agent
           image: log-agent



`back to top <#kubernetes>`_

Services
========

* `NodePort`_, `ClusterIP`_, `LoadBalancer`_, `Headless Services`_
* `Selector-less Services`_, `Connect from External`_
* service-discovery tools help solve the problem of finding which processes are listening at
which addresses for which services
* services enable communications between applications and users
* are loosely coupled and span across multiple nodes and map the ports to the matching pods in
the cluster
* service objects are at Layer 4, only forward TCP and UDP connections without checking

NodePort
--------
    * the service listens on the node and forwards requests to the pod
    * ``targetPort``: port of the pod, omitting will be the same as ``port``
    * ``port``: port of the service, only mandatory field
    * ``nodePort``: port of the node, omitting will be chosen random available free port
    * node port can only be in valid range 30000-32767, omitting will default to random valid
    * uses Random algorithm and has SessionAffinity
    * can use the NodePort without knowing where any of the pods for the service are running

    .. code-block:: yaml

       # service-definition.yml
       apiVersion: v1
       kind: Service
       metadata:
         name: myapp-service
       spec:
         type: NodePort
         ports:
           - targetPort: 80
             port: 80
             nodePort: 30008
         selector:
           # labels of the pod
           app: myapp
           type: front-end


    .. code-block:: sh

       kubectl apply -f service-definition.yml
       kubectl get services
   
       # if using minikube
       minikube service myapp-service --url



ClusterIP
---------
    * default type
    * the service creates internal ip inside the cluster to enable communication
    * group the pods together and provide single interface to access the pods in the group
    * requests are forwarded randomly
    * allows to deploy microservices based application on K8s cluster
    * each layer can scale without impacting the communication
    * each service gets an ip and a name, which are used by pods
    * ``targetPort``: port of the layer (e.g back-end)
    * ``port``: port of the service
    * DNS Service
        - as cluster IP is virtual, it is stable and appropriate to give it a DNS address
        - K8s DNS service is installed as system component when the cluster is created
        - it provides DNS names for cluster IPs
    * address range is configured using ``--service-cluster-ip-range`` flag on the ``kube-apiserver``
    binary
    * older mechanisms inject a set of ENV variables into pods at start up
        - it requires resources to be created in a specific order
        - services must be created before the pods that reference them

    .. code-block:: yaml

       # service-definition.yml
       apiVersion: v1
       kind: Service
       metadata:
         name: back-end
       spec:
         type: ClusterIP
         ports:
           - targetPort: 80
             port: 80
         selector:
           app: myapp
           type: back-end



LoadBalancer
------------
    * distribute load across servers
    * will be the same as ``NodePort`` in an unsupported environment as it is built on the
    ``NodePort`` type by additionally configuring the cloud to create a new load balancer
    * most cloud-based clusters offer load balancer integration
    * external load balancers connect to the public internet
    * use internal load balancers to expose application only within private network which is
    done in ad hoc manner via object annotations
        - Azure, ``service.beta.kubernetes.io/azure-load-balancer-internal: "true"``
        - AWS, ``service.beta.kubernetes.io/aws-load-balancer-internal: "true"``
        - Alibaba, ``service.beta.kubernetes.io/alibaba-cloud-load-balancer-address-type: "intranet"``
        - GCP, ``cloud.google.com/load-balancer-type: "Internal``
    * user can specify a specific cluster IP but once set, cannot be modified without deleting
    and recreating

    .. code-block:: yaml

       apiVersion: v1
       kind: Service
       metadata:
         name: myapp-service
       spec:
         type: LoadBalancer
         ports:
           - targetPort: 80
             port: 80
             nodePort: 30008



Headless Services
-----------------
    * gives DNS entry, created as normal service but does not have IP and does not load balance
    * each pod has DNS in ``podname.headless-servicename.namespace.svc.cluster-domain.example``
    * e.g ``mysql-0.mysql-h.default.svc.cluster.local``
    * ``clusterIP`` is the only difference from normal service
    * gives DNS entry to a pod only if it has ``subdomain`` and ``hostname`` under ``spec``
    * pods created by StatefulSet gets the right ``subdomain`` and ``hostname`` automatically, no
    need to specify in definition file but `serviceName` is required

    .. code-block:: yaml

       # headless-service.yml
       apiVersion: v1
       kind: Service
       metadata:
         name: mysql-h
       spec:
         ports:
         - port: 3306
         selector:
           app: mysql
         clusterIP: None
   
       # pod-definition.yml
       spec:
         subdomain: mysql-h
         hostname: mysql-pod
   
       # deployment-definition.yml
       kind: Deployment
       spec:
         replicas: 3
         template:
           spec:
             # all 3 pods will have same domain name
             subdomain: mysql-h
             hostname: mysql-pod


* ``kubeclt expose deployment myapp`` will pull both the label selector and relevant ports from
the deployment definition
* service object track which pods are ready via a readiness check
* for every Service object, Endpoints object is created that contains the IP addresses for that
service
* as services are built on top of label selectors over pods, K8s API can be used for basic
service discovery without using a Service object, `kubectl get pods -o wide`
* ``kube-proxy`` watches for new services and makes a set of iptables rules in the kernel of the
host to rewrite the destinations of packets to direct them to the endpoints

Selector-less Services
----------------------
    * can be used to declare a K8s service with manually assigned IP address that is outside of
    the cluster
    * service discovery via DNS works as expected
    * to create, remove ``spec.selecotr``field from the resource, while leaving the ``metadata``
    and `ports` section unchanged
    * endpoints must be added manually
    * will need to update the endpoint resource if the IP address changes

        .. code-block:: yaml

           apiVersion: v1
           kind: Endpoints
           metadata:
             name: my-external-server
           subsets:
           - addresses:
             - ip: 1.1.1.1
             ports:
             - port: 3000



Connect from External
---------------------
    * connecting external resources to K8s services is complex
    * if cloud provider supports
          1\. can create an internal load balancer in the virtual private network and deliver
          traffic from a fixed IP address into the cluster and then use DNS to make the IP
          address available to the external resources
                    2\. can use ``NodePort`` service to expose the service on the IP addresses of the nodes in
          the cluster and program a physical load balancer to serve traffic to the nodes or use
          DNS-based load balancing to spread traffic between nodes
    * can run ``kube-proxy`` on the external resource and program it to use the DNS server in the
    cluster
    * other open source solutions, such as HashiCorp Consul, can be used to manage connectivity
    between in-cluster and out-of-cluster resources

`back to top <#kubernetes>`_

Workloads
=========

* `Replication Controller`_, `ReplicaSet`_, `Deployments`_, `Deployment Strategies`_
* `StatefulSets`_, `Daemonset`_, `Jobs`_, `CronJobs`_

Replication Controller
----------------------
    * helps run multiple instances of a single pod in a cluster for high availability
    * help renew a single pod if the existing one fails, can have self-healing applications
    * ensure the specified number of pods are running
    * load balance and scale based on demand across multiple nodes in a cluster
    * will not deploy already matching label pods
    * replaced by ReplicaSet

    .. code-block:: yaml

       # rc-definition.yml
       apiVersion: v1
       kind: ReplicationController
       metadata:
         name: myapp-rc
         labels:
           app: myapp
           type: front-end
       spec:
         template:
   
           # pod
           metadata:
             name: myapp-pod
             labels:
               app: myapp
               type: front-end
           spec:
             containers:
             - name: nginx-container
               image: nginx
   
         replicas: 3


    .. code-block:: sh

       kubectl apply -f rc-definition.yml
   
       kubectl get replicationcontroller
   
       # will see 3 pods
       kubectl get pods



ReplicaSet
----------
    * to manage pods, new recommended way to setup replication, self-healing applications
    * ensure the desired number of pods are running
    * can have ``spec.selector`` to identify what pods fall under it and ``selector`` should be
    a subset of the labels in the pod template
    * ``selector`` is the major difference from ``ReplicationController``, helps filter resources
    * can make a ReplicaSet to use the existing pod and scale
    * can modify the labels on the pod to disassociate from the ReplicaSet for debugging
    * ``annotations`` are used to record details for information purpose
    * always add ``template`` section even if pods are already created to maintain availability
    in the future
    * will not allow pods with the same label to be created again and terminate them if created
    * ReplicaSets are for stateless services and every pod created is homogeneous
    * arbitrary pod is selected for scaling and application's behavior should not change when
    scaled
    * most use ReplicSets as default instead of pods, even for single pod

    .. code-block:: yaml

       # replicaset-definition.yml
       apiVersion: apps/v1
       kind: ReplicaSet
       metadata:
         name: myapp-replicaset
         labels:
           app: myapp
           type: front-end
         annotations:
           buildVersion: 1.1
       spec:
         template:
   
           # pod
           metadata:
             name: myapp-pod
             labels:
               app: myapp
               type: front-end
           spec:
             containers:
             - name: nginx-container
               image: nginx
   
         replicas: 3
         selector:
           # labels of pod
           matchLabels:
             type: front-end


    .. code-block:: sh

       kubectl apply -f replicaset-definition.yml
   
       kubectl get replicasets
   
       kubectl get pods
       kubectl get po --selector type=front-end --no-headers | wc -l
       kubectl get po -l type=front-end,app=myapp
   
       # delete only ReplicaSet object, not the pods
       kubectl delete rs myapp-replicaset --cascade=false


    * can edit ``replicas`` in the yml file to update and scale
        - always make sure to update changes in yml file if updated with imperative commands

        .. code-block:: sh

           kubectl replace -f replicaset-definition.yml
           # OR (will not update the file)
           kubectl scale --replicas=6 -f replicaset-definition.yml
           # OR (will not update the file)
           kubectl scale --replicas=6 replicaset myapp-replicaset
           # OR (will open temp file to edit config)
           kubectl edit replicasets myapp-replicaset
   
           kubectl describe replicaset myapp-replicaset


        .. code-block:: yaml

           # pod-definition.yml
           apiVersion: v1
           kind: Pod
           metadata:
             name: math-pod
           spec:
             containers:
             - name: math-add
               image: ubuntu
               command: ['expr', '3', '+', '2']


    * HPA (Horizontal Pod Autoscaling)
        - horizontal scaling: creating more pod replicas
        - vertical scaling: increasing resources required for a pod
        - need ``metrics-server`` in the cluster for auto scaling
        - scaling based on CPU usage is most common
        - not recommended to combine autoscaling with management of number of replicas

        .. code-block:: sh

           kubectl autoscale --min=2 --max=4 --cpu-percent=80 rs myapp-replicaset
   
           # can use hpa resource
           kubectl get hpa


    * pod will restart again and again until threshold is reached as the policy is to restart
    always by default

        .. code-block:: yaml

           # pod-definition.yml
           spec:
             restartPolicy: Always
             # OR
             restartPolicy: Never
             # OR
             restartPolicy: Failure


    * pods created will have ``ownerReferences`` section and can be used to check which
    ReplicaSet manages

        .. code-block:: sh

           kubectl get pods my-pod -o jsonpath='{.metadata.ownerReferences[0].name}'



Deployments
-----------
    * manages ReplicaSets, object above ReplicaSet in hierarchy
    * can update underlying instances with rolling-update, undo and resume changes
    * uses health checks on new versions and stops if many failures occur
    * **Creating Deployments**
        - using YAML file

        .. code-block:: yaml

           # deployment-definition.yml
           apiVersion: apps/v1
           kind: Deployment
           metadata:
             name: myapp-deployment
             labels:
               app: myapp
               type: front-end
           spec:
             template:
   
               # pod
               metadata:
                 name: myapp-pod
                 labels:
                   app: myapp
                   type: front-end
               spec:
                 containers:
                 - name: nginx-container
                   image: nginx
   
             replicas: 3
             selector:
               # labels of pod
               matchLabels:
                 type: front-end


        .. code-block:: sh

           kubectl apply -f deployment-definition.yml
   
           kubectl get deployments
   
           # deployment create replicaset
           kubectl get rs --selector=type=front-end
   
           # replicaset create pods
           kubectl get pods
   
           # get all objects
           kubectl get all


    * **Editing Deployments**
        - can easily edit any field/property of the pod templates
        - every change in Deployment will auto delete and create a new pod
        - scaling the Deployment also scales the ReplicaSet, but not vice-versa
        - need to delete the Deployment with ``--cascade=false`` to manage the ReplicaSet directly

        .. code-block:: sh

           kubectl edit deploy myapp-deployment
   
           kubeclt scale deployments myapp-deployment --replicas=3


    * **Rollout**
        - when new Deployment is created, a new rollout is triggered, which creates a new
          revision
        - updating will trigger a new rollout and revision
        - ``OldReplicaSets`` and ``NewReplicaSet`` fields are set to values in the middle of the
          rollout
        - annotations can be used to record information about the update
        - do not update ``change-cause`` annotation when simple scaling
        - can undo regardless of the rollout stage
        - recommended to edit YAML files to revert versions, rather than imperative ``undo``
        - roll back uses the previous template and renumbers it as the latest revision
        - last 10 revisions are kept by default and can set maximum history size with
          ``spec.revisionHistoryLimit``

        .. code-block:: sh

           # record change cause
           kubectl apply -f deployment-definition.yml --record
   
           # after editing definition file
           kubectl apply -f deployment-definition.yml --record
   
           # or update from command
           kubectl set image deployment nginx nginx=nginx:1.18
   
           # current status
           kubectl rollout status deployment myapp-deployment
   
           kubectl rollout pause deployment myapp-deployment
           kubectl rollout resume deployment myapp-deployment
   
           # oldest to newest older
           kubectl rollout history deployment myapp-deployment
           # specific revision
           kubectl rollout history deployment myapp-deployment --revision=1
   
           kubectl rollout undo deployment myapp-deployment
   
           # rollback to specific revision
           kubectl rollout undo deployment myapp-deployment --to-revision=3
           # same as 'kubectl rollout undo'
           kubectl rollout undo deployment myapp-deployment --to-revision=0


    * health checks
        - set ``spec.minReadySeconds`` to make the Deployment wait to update next pod after
          current pod passes health check
        - set ``spec.progessDeadlineSeconds`` to time out the rollout and mark it as failed when
          there's a bug and to avoid the Deployment being stalled forever
        - timeout is defined as the time the Deployment creates or deletes a pod, not overall
          length of a Deployment
        - check ``status.conditions`` to check the Deployment state

Deployment Strategies
---------------------
    * applications should be able to communicate with slightly older and newer versions
    * should maintain backward and forward compatibility for reliable updates
    * **Recreate**
        - destroy all old versions first and create new versions
        - fast and simple, but workload downtime exists

        .. code-block:: yaml

           # deployment-definition.yml
           spec:
             replicas: 3
             selector:
               matchLabels:
                 type: front-end
             strategy:
               type: Recreate


    * **RollingUpdate**
        - default strategy, recommended for user-facing services
        - destroy old versions and create new versions one by one
        - ``Deployment`` create a new ``replicaset`` and upgrade one after another
        - ``kubectl get rs`` will show two ``replicasets``
        - ``maxUnavailable`` sets the max pods that can be unavailable during update and allows
          to trade speed for availability (Recreate strategy can be considered as
          ``maxUnavailable`` set to 100%)
        - can set ``maxUnavailable`` to 0 and use ``maxSurge`` to maintain 100% capacity and just
          use extra resources for rollout
        - ``maxSurge`` controls how many extra resources can be created for a rollout (setting
          it to 100% is same a blue/green strategy)
        - using percentage to set parameters is good approach

        .. code-block:: yaml

           # deployment-definition.yml
           spec:
             replicas: 3
             selector:
               matchLabels:
                 type: front-end
             strategy:
               rollingUpdate:
                 maxSurge: 25%
                 maxUnavailable: 25%
               type: RollingUpdate


    * **Blue/Green**
        - new version (Green) is deployed alongside the old version (Blue)
        - all traffic is still routed to blue
        - deploy the new version first and perform necessary tests
        - once all tests are passed on green, all traffic is routed to green all at once

        .. code-block:: yaml

           # blue.yml
           kind: Deployment
           spec:
             selector:
               matchingLabels:
                 version: v1
   
           # green.yml
           kind: Deployment
           spec:
             selector:
               matchingLabels:
                 version: v2
   
           # service-definition.yml
           kind: Service
           spec:
             selector:
               version: v1 # old version
               # change once new version is ready
               version: v2


    * **Canary**
        - deploy a new version, canary, and route only small % of traffic to it
        - most traffic will still be routed to old version, primary
        - once all tests passed on new version, deployment will be upgraded
        - the canary deployment created earlier is then deleted
        - set same label on both primary and canary, and set selector of service to that
        - to only route small % of traffic to canary, reduce the number of pods on it
        - as service distribute traffic equally, most will still go to primary
        - doing this way limits the control of traffic on the deployments
        - service meshes, like Istio, comes with better control, exact % can be defined

        .. code-block:: yaml

           # primary.yml
           kind: Deployment
           spec:
             replicas: 5
             selector:
               matchLabels:
                 app: front-end
   
           # canary.yml
           kind: Deployment
           spec:
             replicas: 1
             selector:
               matchLabels:
                 app: front-end
   
           # service-definition.yml
           kind: Service
           spec:
             selector:
               app: front-end # use same label on both


    .. code-block:: sh

       # edit deployment-definition.yml file, will trigger new rollout
       kubectl apply -f deployment-definition.yml
   
       # will make new rollout but the deployment-definition.yml file will not be updated
       kubectl set image deployment/myapp-deployment nginx-container=nginx:NEW_VERSION
   
       # will also show deployment strategy
       kubectl describe myapp-deployment



StatefulSets
------------
    * similar to Deployments, create pods based on templates, scale, rollingupdate and rollback
    * main feature is that pods are created in sequential order
    * first pod must be in running and ready state before next one is deployed
    * assigns unique ordinal index to each pod
    * each pod gets a unique name, combining index and statefulset name, no more random names
    * always has same name even if the pod is restarted
    * ``serviceName``: name of headless service
    * ``podManagementPolicy``: change ordering policy, ``OrderedReady`` (default), ``Parallel``

    .. code-block:: yaml

       # statefulset-definition.yml
       apiVersion: apps/v1
       kind: StatefulSet
       metadata:
         name: mysql
         labels:
           app: mysql
       spec:
         template:
           metadata:
             labels:
               app: mysql
           spec:
             containers:
             - name: mysql
               image: mysql
         replicas: 3
         selector:
           matchLabels:
             app: mysql
         serviceName: mysql-h
         podManagementPolicy: Parallel # will not scale or delete in order


    .. code-block:: sh

       # create pods one after another
       kubectl apply -f statefulset-definition.yml
   
       # scale up in order
       kubectl scale statefulset mysql --replicas=5
   
       # scale down in reverse order
       kubectl scale statefulset mysql --replicas=3
   
       # delete in reverse order
       kubectl delete statefulset mysql


    * if volume is specified, all pods created by StatefulSet will try to use that volume
    * **volumeClaimTemplates**
        - is a PVC (Persistent Volume), ensure each pod created by StatefulSet gets a
          individual PVC (Persistent Volume Claim)
        - defined in statefulset-definition.yml file under ``spec``
        - during the creation of a pod, PVC is created and associated to a SC (Storage Class),
          the SC creates PV and binds the PVC to the PV
        - StatefulSet do not auto delete the PVC and volume if the associated pod fails
        - instead makes sure that the restarted pod is attached to the same PVC before

        .. code-block:: yaml

           # statefulset-definition.yml
           spec:
             volumeClaimTemplates:
             - metadata:
                 name: data-volume
               spec:
                 accessModes:
                   - ReadWriteOnce
                 storageClassName: google-storage
                 resources:
                   requests:
                     storage: 500Mi



Daemonset
---------
    * like replica set, helps to deploy multiple instances of pod, created by DaemonSet
    controller through kube-apiserver
    * ensures one copy of the pod is always present on all nodes in the cluster
    * when a new node is added to the cluster, a replica of the pod is auto added to that node
    * the pod is removed when the node removes
    * e.g deploying monitoring and logging agents, kube-proxy, networking solutions like
    Weave-net
    * from v1.12, uses nodeAffinity and default scheduler to schedule pods
    * definition file is similar to ReplicaSets

    .. code-block:: yaml

       # daemon-set-definition.yml
       apiVersion: apps/v1
       kind: DaemonSet
       metadata:
         name: monitoring-daemon
       spec:
         selector:
           matchLabels:
             app: monitoring-agent
         template:
           metadata:
             labels:
               app: monitoring-agent
           spec:
             containers:
             - name: monitoring-agent
               image: monitoring-agent


    .. code-block:: sh

       kubectl apply -f daemon-set-definition.yml
       kubectl get daemonsets
       kubectl describe ds monitoring-daemon



Jobs
----
    * in batch processing, pods need perform the task in parallel and exit once done
    * need a manager to create number of desired pods and ensure the task is done successfully
    * ``ReplicaSets`` only make sure specified number of pods are running at all time
    * ``Job`` is used to run a set of pods to perform the task to completion
    * output of a job is in stdout for simple tasks
    * tasks such as image processing will need a volume to store the output

    .. code-block:: yaml

       # job-definition.yml
       apiVersion: batch/v1
       kind: Job
       metadata:
         name: math-add-job
       spec:
         template:
           spec:
             containers:
             - name: math-add
               image: ubuntu
               command: ['expr', '3', '+', '2']
             restartPolicy: Never


    .. code-block:: sh

       # create job
       kubectl apply -f job-definition.yml
   
       kubectl get jobs
   
       # see output
       kubectl logs math-add-job-pod


    * run multiple jobs
        - second pod is created only after first finishes
        - will create new pods until desired completions is met

        .. code-block:: yaml

           # job-definition.yml
           spec:
             completions: 3


    * create pods in parallel
        - will create new pods until desired completions is met
        - intelligent enough to know how many pods will have to be created

        .. code-block:: yaml

           # job-definition.yml
           spec:
             completions: 3
             parallelism: 3



CronJobs
--------
    * scheduled a job and run periodically

    .. code-block:: yaml

       # cron-job-definition.yml
       apiVersion: batch/v1
       kind: CronJob
       metadata:
         name: cron-job
       spec: # CronJob spec
         schedule: "*/1 * * * *"
         jobTemplate:
           spec: # Job spec
             completions: 3
             parallelism: 3
             template:
               spec: # Pod spec
                 containers:
                 - name: math-add
                   image: ubuntu
                   command: ['expr', '3', '+', '2']
                 restartPolicy: Never


    .. code-block:: sh

       # create cron job
       kubectl apply -f job-definition.yml
   
       kubectl get cronjobs


`back to top <#kubernetes>`_

Storage & Volumes
=================

* `Recommended Patterns`_, `Persistent Volume`_, `Persistent Volume Claims`_, `Storage Classes`_
* containers and pods are transient by nature
* to have data persistence, volumes must be created and mounted to a pod

Recommended Patterns
--------------------
    * Communication/Synchronization
        - e.g using ``emptyDir`` volume which only has the scope of pod's lifespan but can be
          shared between two containers
    * Cache
        - cache must survive a container restart and ``emptyDir`` can also be used
    * Persistent data
        - independent of the lifespan of a pod
        - e.g NFS, iSCSI, cloud provider storage
    * Mounting the host filesystem
        - ``hostPath`` volume, which can mount arbitrary locations on the worker node into the
          container
        - not recommended unless there are specific reasons
* external replicated cluster storage solutions: NFS, GlusterFS, Flocker, Ceph, ScaleIO,
AWS EBS, Azuer Disk, GCE Persistence Disk

.. code-block:: yaml

   # pod-definition.yml
   apiVersion: v1
   kind: Pod
   metadata:
     name: myapp-pod
   spec:
     containers:
     - name: myapp-pod
       image: ubuntu
       command: ["echo"]
       args: ["hello >> /tmp/hello.txt"]
       volumeMounts:
       - mountPath: /tmp
         name: data-volume
     volumes:
     - name: data-volume
       hostPath: # not ideal for multiple nodes
         path: /data
         type: Directory
       awsElasticBlockStore:
         volumeID: VOLUME_ID
         fsType: ext4


* putting volume configurations in a pod-definition.yml raises the issue to add volume
configuration in every pod definition file

Persistent Volume (PV)
----------------------
    * can manage volume centrally, configured by an admin
    * ``accessModes``: how a volume should be mounted on a host, ``ReadOnlyMany``, ``ReadWriteOnce``,
    ``ReadWriteMany``
    * ``persistentVolumeReclaimPolicy``: define what to do when the PVC is deleted, ``Retain``,
    ``Delete``, ``Recycle``

    .. code-block:: yaml

       # pv-definition.yml
       apiVersion: v1
       kind: PersistentVolume
       metadata:
         name: pv-vol1
       spec:
         accessModes:
           - ReadWriteOnce
         capacity:
           storage: 1Gi
         hostPath: # not for production
           path: /tmp/data
         awsElasticBlockStore:
           volumeID: VOLUME_ID
           fsType: ext4


    .. code-block:: sh

       kubectl apply -f pv-definition.yml
       kubectl get persistentvolume



Persistent Volume Claims (PVC)
------------------------------
    * to use from a PV, configured by a user
    * every PVC is bound to a PV, one-to-one relationship between claim and volume
    * K8s binds the PV based on requests and properties set on the volumes such as sufficient
    capacity, access modes, volume modes, storage class, selector
    * smaller claims can be bound to larger volumes if no other matches available, other claims
    cannot use the remaining volume
    * PVC will remain pending until new volumes are available
    * will stuck in terminating state if the pvc is deleted while the pod is using

    .. code-block:: yaml

       # pvc-definition.yml
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: myclaim
       spec:
         accessModes:
           - ReadWriteOnce
         resources:
           requests:
             storage: 500Mi


    .. code-block:: sh

       kubectl apply -f pvc-definition.yml
       kubectl get persistentvolumeclaim
       kubectl delete pvc myclaim


    .. code-block:: yaml

       # pod-definition.yml
       spec:
         volumes:
         - name: mypd
           persistentVolumeClaim:
             claimName: myclaim


* static provisioning: when creating pvc from cloud, the volume has to be manually configured
on the cloud first

Storage Classes
---------------
    * auto provision storage when needed, dynamic provisioning
    * PV definition file is not needed as it will be auto created by SC

    .. code-block:: yaml

       # sc-definition.yml
       apiVersion: storage.k8s.io/v1
       kind: StorageClass
       metadata:
         name: google-storage
       provisioner: kubernetes.io/gce-pd
       parameters: # provider specific
         type: pd-standard
         replication-type: none
   
       # pvc-definition.yml
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: myclaim
       spec:
         accessModes:
           - ReadWriteOnce
         storageClassName: google-storage
         resources:
           requests:
             storage: 500Mi


`back to top <#kubernetes>`_

Controller
==========

* `Kube Controller Manager`_, `Node Controller`_, `Admission Controller`_
* `Webhook`_, `Custom Controllers`_, `Operator Framework`_

Kube Controller Manager
-----------------------
    * single process that manages various controllers in K8s
    * can download from K8s release page
    * kubeadm deploy as pod in kube-system namespace, definition file in
    'etc/kubernetes/manifests/kube-controller-manager.yml'
    * can be searched as a process ``ps -aux | grep kube-controller-manager`` on the controlplane node
    * in non kubeadm setup, '/etc/systemd/system/kube-controller-manager.service'

Node Controller
---------------
    * responsible for monitoring status of the nodes
    * take necessary actions to keep the applications running through kube-apiserver
    * get status of the nodes every 5s
    * wait for 40s before marking a node unreachable
    * gives a node 5 min to comeback up
    * if the node doesn't comeback, it removes the pod on that node and assign it to the
    healthy node if the pod is part of replicaset

Admission Controller
--------------------
    * most rules created with RBAC are at API level
    * Admission Controllers can enforce how a cluster is used, can do more than authorization
    * built-ins such AlwaysPullImages, DefaultStorageClass, EventRateLimit, NamespaceExists
    * NamespaceExists
        - check if the specified namespace exists in the request
        - enabled by default
    * NamespaceAutoProvision
        - not enabled by default
        - auto create namespace if it doesn't exist
    * can perform operations in the backend and change the request
    * NamespaceExists and NamespaceAutoProvision are deprecated and replaced by
    NamespaceLifecycle
    * ``--disable-admission-plugins`` to disable

    .. code-block:: sh

       # view enabled admission controllers
       kube-apiserver -h | grep enable-admission-plugins
       grep enable-admission-plugins /etc/kubernetes/manifests/kube-apiserver.yaml
   
       # if using kubeadm setup
       kubectl exec kube-apiserver-controlplane -n kube-system -- kube-apiserver -h
       ps -ef | grep kube-apiserver | grep admission-plugins


    * validating admission controller
        - validates the request and allow or deny
        - e.g ``NamespaceExists`` controller
    * mutating admission controller
        - mutates the request and performs it
        - e.g ``DefaultStorageClass`` controller
    * mutate first, then validate

Webhook
-------
    * custom admission controller
    * e.g MutatingAdmission Webhook, ValidatingAdmission Webhook
    * can be configured to point a server within or outside the cluster
    * after the request go through all built-in controllers, it gets to the Webhook and makes
    a call to admission webhook server by passing object in json
    * Admission Webhook server responds with Admission Review object
    * requirement to be a Webhook is to accept the mutate, validate and respond with json object
    * need a webhook service if deployed in K8s cluster

        .. code-block:: yaml

           # webhook.yml
           apiVersion: admissionregistration.k8s.io/v1
           kind: ValidatingWebhookConfiguration
           metadata:
             name: "pod-policy.example.com"
           webhooks:
           - name: "pod-policy.example.com"
             clientConfig: # location of webhook server
               url: "https://external-server.com" # if deployed external
               service: # if in the cluster
                 namespace: "webhook-namespace"
                 name: "webhook-service"
               caBundle: "fdsafdsafsaf...TLS"
             rules:
             - apiGroups: [""]
               apiVersions: ["v1"]
               operations: ["CREATE"]
               resources: ["pods"]
               scope: "Namespaced"



Custom Controllers
------------------
    * monitor custom objects in etcd and handle them
    * can use Python but will be hard to configure such as queuing, caching
    * developing in K8s Go client have support for other libraries
    * can package the controller in docker image and run it inside K8s cluster
    * [sample controller](https://github.com/kubernetes/sample-controller)

    .. code-block:: sh

       go build -o sample-controller .
   
       # to authenticate to K8s API
       ./sample-controller --kubeconfig=$HOME/.kube/config



Operator Framework
------------------
    * package CRD and Custom Controller and deploy as single entity
    * etcd operator
        - popular operator framework to deploy and manage etcd cluster within K8s
        - has EtcdCluster CRD and ETCD Controller
        - can also take backup (EtcdBackup, Backup Operator), restore (EtcdRestore, Restore
          Operator)
    * [OperatorHub](https://operatorhub.io/)

`back to top <#kubernetes>`_

Scheduler
=========

* `Binding`_, `Multiple Schedulers`_
* looks at each pod and try to find the best node
* first filter the nodes that do not fit the pod
* from the remaining nodes, it calculates amount of resources that will be free on the
node after the pod is placed
* place the pod on the node that will leave more free resource
* can make custom scheduler
* can download from K8s releases and run it as a service
* kubeadm deploy as pod in kube-system namespace, definition file in
'etc/kubernetes/manifests/kube-scheduler.yml'
* can be searched as a process ``ps -aux | grep kube-scheduler`` on the controlplane node
* in non kubeadm setup, '/etc/systemd/system/kube-scheduler.service'
* every pod has a ``nodeName`` field under ``spec`` that is not set by default
* the scheduler looks for candidate pods, that do not have ``nodeName`` field, to schedule
* then schedule the pods by setting the ``nodeName`` to the name of the node, creating binding
object
* the pods will be in pending state if there is no scheduler
* easiest way to schedule a pod is to set the ``nodeName`` field to the name of the node
* can only specify ``nodeName`` at creation time
* cannot modify the ``nodeName`` property of a created pod

Binding
-------
    * to assign a node to an existing pod and manually schedule a pod
    * create the object and send a request to the pod's binding API
    * same method as the scheduler does

    .. code-block:: yaml

       # pod-bind-definition.yml
       apiVersion: v1
       kind: Binding
       metadata:
         name: nginx
       target:
         apiVersion: v1
         kind: Node
         name: node02


    .. code-block:: sh

       # YAML must be converted to JSON format first
       curl --header "Content-Type:application/json" --request POST --data '{YAML data in JSON}'\
       http://$SERVER/api/v1/namespaces/default/pods/$PODNAME/binding


* can have **multiple schedulers** at the same time <a id="multiple-schedulers"></a>
    * can pass ``--scheduler-name`` options in kube-scheduler
    * ``leader-elect``, use when multiple copies of the scheduler on different controlplane nodes
    * ``lock-object-name``, to differentiate custom scheduler from the default one during leader
    election process
    * pod will be in pending state if the scheduler is not configured correctly

    ```custom-scheduler.yml
    apiVersion: v1
    kind: Pod
    metadata:
      name: custom-scheduler
      namespace: kube-system
    spec:
      containers:
      * command:
        * kube-scheduler
        * --address=127.0.0.1
        * --kubeconfig=/etc/kubernetes/scheduler.conf
        * --leader-elect=true
        * --scheduler-name=custom-scheduler
        * --lock-object-name=custom-scheduler
        image: k8s.gcr.io/kube-scheduler-amdd64
        name: kube-scheduler

    # pod-definition.yml
    apiVersion: v1
    kind: Pod
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
      * name: nginx-container
        image: nginx
      schedulerName: custom-scheduler


* `Scheduler code hierarchy overview`_, `Advanced Scheduling`_
* How the Scheduler works: [jvns](https://jvns.ca/blog/2017/07/27/how-does-the-kubernetes-scheduler-work/), [stack over flow](https://stackoverflow.com/questions/28857993/how-does-kubernetes-scheduler-work)

`back to top <#kubernetes>`_

Networking
==========

* `Ports`_, `Implementing Network Model`_, `CNI in K8s`_, `Service Networking`_
* `DNS in K8s`_, `Network Policies`_, `Ingress`_, `mTLS`_
* IP addresses are assigned to pods, each pod has its own IP address
* K8s creates internal private network when configured initially

ports
-----
    * kube-apiserver: 6443
    * kubelet: 10250
    * kube-scheduler: 10251
    * kube-controller-manager: 10252
    * etcd: 2379 (also need 2380 if multiple control-plane nodes)
    * worker nodes expose servcies on 30000-32767
* accessing a pod with its internal IP is not ideal
* internal network addresses of different nodes, which are not in a cluster, can be same
* K8s does not auto setup for IP conflicts, users must setup on their own
    * all containers/pods must be able to communicate one another without NAT
    * all nodes must be able to communicate with all containers and vice-versa without NAT
    * the IP that a container sees itself as is the same IP that others see it as
* can use [prebuilt](https://kubernetes.io/docs/concepts/cluster-administration/addons/#networking-and-network-policy) solutions to setup network
    * e.g Flannel or Calico for setting up from scratch, NSX for VMware environment

Implementing Network Model
--------------------------
    * K8s creates network namespaces for containers automatically
    * create bridge network on each node, bring them up
    * assign IP addresses to bridge interfaces/networks, by choosing any private address range
    * create a script to be used every time a container is created, with necessary parameters
    * use a router as default gateway for all hosts to use and manage the routing table on it,
    no need to configure routes on each container


    # net-script.sh

    # create veth pair
    ip link add ...

    # attach veth pair
    ip link set ...
    ip link set ...

    # assign IP address
    ip -n ... addr add ...
    ip -n ... addr add ...

    # bring up interface
    ip -n ... link set ...


    * CNI tells K8s to call the script as soon as the container is created
    * the script should meet CNI standards
    * when a container is created, kubelet looks for script name in ``--cni-conf-dir`` and
    find the script in `--cin-bin-dir` and run it with `./net-script.sh add cid ns`


        # net-script.sh

        ADD)
            # create veth pair
            # attach veth pair
            # assign ip address
            # bring up interface

        DEL)
            # delete veth pari
            ip link del ...



CNI in K8s
----------
    * CNI plugin must be invoke by the component that is responsible for creating containers
    * plugin is configured in ``kubelet.service`` on each node in the cluster
    * ``--cni-bin-dir=/opt/cni/bin`` has all executable plugins
    * ``--cni-conf-dir=/etc/cni/net.d`` has configuration files which kubelet looks for
    * host-local
        - plugin that manages IP addresses locally on each host
        - can be specified in ``ipam`` of ``bridge.conf`` file


    # bridge.conf

    {
        "cniVersion":,
        "name":,
        "type":,
        "bridge":,
        "isGateway":,
        "ipMasq":,
        "ipam": {
            "type":,
            "subnet":,
            "routes": [{}]
        },
    }



Service Networking
------------------
    * when a service is created, it is accessible by all pods on the cluster
    * there are no processes, namespaces or interfaces for services
    * service is just a virtual object, assigned with IP address within predefined range, in
    ``--service-cluster-ip-range``, which defaults to 10.0.0.0/24
    * kube-proxy gets the IP address and creates forwarding rules in the cluster
    * the rules has same lifecycle as the services
    * kube-proxy create rules using userspace, ipvs or iptables (default)
    * entries can be found in ``/var/log/kube-proxy.log``

    .. code-block:: sh

       # check ip range
       ps -aux | grep kube-apiserver
   
       # check the rules created
       iptables -L -t nat | gre my-service



DNS in K8s
----------
    * K8s deploy built-in DNS server by default
    * coredns is recommended, kube-dns was used before
    * **services**
        - when a service is created, DNS service auto creates a record for it so that any pod
          can reach the service using the service name
        - ``cluster.local``: domain name
        - ``svc``: sub domain for service
        - ``dev``: namespace
        - ``my-service``: service name
        - if same namespace, the service can be easily reached by using its name, ``myservice``
        - for separate namespace, ``my-service.namespace1``
        - all services are grouped together into ``svc`` subdomain
        - all services and pods are grouped together into ``cluster.local`` root domain
        - full domain name of a service, ``my-service.namespace1.svc.cluster.local``
    * **pods**
        - records for pods are not created by default
        - full domain name of a pod, ``10-244-2-5.namespace1.type.cluster.local``, dots in pod's
          IP are replaced by dashes
    * **coredns**
        - deployed as pod which runs Coredns executable
        - K8s uses ``/etc/coredns/Corefile`` for configuration file
        - multiple plugins are enabled in the file
        - ``kubernetes`` plugin makes CoreDNS work with K8s, where top level domain name of the
          cluster is set
        - ``pods`` options in the file is responsible for creating records for pods
        - any record that the DNS server can't solve is forwarded to the nameserver specified
          in ``proxy`` in the Corefile
        - the Corefile is also passed in to the pods as configmap object
        - when coredns is deployed as pod, a service called ``kube-dns`` is also deployed
        - IP of kube-dns is configured as nameserver and default search entry is also added in
          ``/etc/resolv.conf`` on pods, which is done by kubelet automatically
        - IP of DNS server and domain can be found in ``/var/lib/kubelet/config.yml``

Network Policies
----------------
    * K8s allow all traffic from all pods to all destinations by default
    * only look the direction in which the traffic originated when defining Ingress (incoming
    traffic) and Egress (outgoing traffic)
    * attach a policy to one or more pods, defining rules within the policy
    * e.g allow ingress traffic from API pod on port 3306
    * Kube-router, Calico, Romana and Weave-net support network policies
    * Flannel does not support network policies
    * omitting ``ingress:`` or ``egress:`` fields under ``policyTypes`` will block all traffic
    * allowing ingress will auto allow the db-pod to be able to respond back but does not allow
    it to make requests to the api-pod, as it is egress rule

    .. code-block:: yaml

       # network-policy.yml
       apiVersion: networking.k8s.io/v1
       kind: NetworkPolicy
       metadata:
         name: db-policy
         namespace: prod # policy for prod namespace
       spec:
         podSelector:
           matchLabels:
             role: db
         policyTypes:
         - Ingress
         - Egress
         ingress:
         - from:
           - podSelector:
               matchLabels:
                 name: api-pod
             namespaceSelector: # omit if same namespace as policy
               matchLabels:
                 name: prod # allow from api-pod on prod
           - ipBlock: # allow from outside the cluster
               cidr: 192.168.5.10/32
           ports: # port on db-pod
           - protocol: TCP
             port: 3306
         egress: # allow to make requests
         - to:
           - ipBlock:
               cidr: 192.168.5.10/32
           ports: # port to which the request will be made
           - protocol: TCP
             port: 80


    * important to understand how to define rules based on requirements

    .. code-block:: yaml

       # allow when podSelector AND namespaceSelector are true
       - podSelector:
           matchLabels:
             name: api-pod
         namespaceSelector:
           matchLabels:
             name: prod
   
       # allow when podSelector OR namespaceSelector is true
       - podSelector:
           matchLabels:
             name: api-pod
       - namespaceSelector:
           matchLabels:
             name: prod


    .. code-block:: sh

       kubectl apply -f network-policy.yml
       kubectl get networkpolicies
       kubectl describe netpol


* virtual hosting: host HTTP sites on single IP address using a load balancer or reverse proxy

Ingress
-------
    * helps users access the application using single accessible URL, HTTP-based load balancing
    * built-in layer 7 loadbalancer and standardizes using K8s primitives and merging multiple
    Ingress objects into single config
    * route within the K8s cluster and implement SSL security
    * only need to expose once
    * **Ingress Controller**
        - **Ingress proxy**: exposed outside the cluster using ``LoadBalancer`` type service
        - **Ingress reconciler/operator**: read and monitor ingress objects and reconfigure
          Ingress proxy to route traffic
        - the solution deployed, does not come with K8s by default
        - must be deployed using third-party as no single load balancer can be set as standard
          and Ingress was added to K8s before other extensibility capabilities
        - GCE and Nginx are supported and maintained by K8s
        - need a service to expose the ingress controller
        - intelligent enough to monitor cluster's ingress resources and configure underlying
          nginx server if needed, but requires a ``ServiceAccount`` with right permissions

        .. code-block:: yaml

           # nginx-ingress-controller.yml
           apiVersion: apps/v1
           kind: Deployment
           metadata:
             name: nginx-ingress-controller
           spec:
             replicas: 1
             selector:
               matchLabels:
                 name: nginx-ingress
             template:
               metadata:
                 labels:
                   name: nginx-ingress
               spec:
                 containersc
                 - name: nginx-ingress-controller
                   image: registry.k8s.io/ingress-nginx/controller:v1.3.1
                 args:
                 - /nginx-ingress-controller
                 # creating ConfigMap object makes easier to manage configuration data
                 - --configmap=$(POD_NAMESPACE)/nginx-configuration
                 env:
                 - name: POD_NAME
                  valueFrom:
                    fieldRef:
                      fieldPath: metadata.name
                 - name: POD_NAMESPACE
                   valueFrom:
                     fieldRef:
                       fieldPath: metadata.namespace
                 ports: # ports used by controller
                 - name: http
                   containerPort: 80
                 - name: https
                   containerPort: 443
   
           # nginx-ingress-configmap.yml
           apiVersion: v1
           kind: ConfigMap
           metadata:
             name: nginx-configuration
   
           # nginx-ingress-service.yml
           apiVersion: v1
           kind: Service
           metadata:
             name: nginx-ingress
           spec:
             type: NodePort
             ports:
             - port: 80
               targetPort: 80
               protocol: TCP
               name: http
             - port: 443
               targetPort: 443
               protocol: TCP
               name: https
             selector:
               name: nginx-ingress
   
           # nginx-ingress-serviceaccount.yml
           apiVersion: v1
           kind: ServiceAccount
           metadata:
             name: nginx-ingress-serviceaccount
           # with correct Roles, ClusterRoles and RoleBindings


    * neeed to configure DNS entries to external address for load balancer
    * can map multiple hostnames to single external endpoint
    * can just pass everything to an upstream service
    * **Ingress Resources**
        - set of rules applied on ingress controller
        - can set rules on each pod based on URL
        - has a ``Default backend`` where the user will be routed to if rules are not matched
        - should deploy a service for ``Default backend``
        - ``paths``: to direct traffic based on the path in HTTP request, can be used to host
          multiple services on different paths of single domain, longest prefix matches if
          multiple paths are on the same host
        - add ``host:`` in each rule to split traffic by domain name and route appropriately
        - ``rewrite-target``: to write and forward internally to the correct URL on the
          application, rewrites the URL by replacing the defined ``rule->http->paths->path``
        - ``spec.ingressClassName``: to enable single Ingress resource to request particular
          implementation and allowing to run multiple Ingress controllers on single cluster,
          default controller will be used if not specified
        - ``nginx.ingress.kubernetes.io/rewrite-target``, modify/rewrite path in the proxied
          request with an annotation on the Ingress object, which applies to all requests
          specified by that object and can make upstream services work on a subpath
        - can also use regular expressions when rewriting path
        - better to avoid path rewriting complicated applications as it can lead to bugs

        .. code-block:: yaml

           # Ingress-wear.yml
           apiVersion: networking.k8s.io/v1
           kind: Ingress
           metadata:
             name: ingress-wear
             annotations:
               nginx.ingress.kubernetes.io/rewrite-target: /
           spec:
             rules:
             - host: domain1.com # split traffic by domain name
               http:
                 paths:
                 - path: /route1
                   backend: # where traffic will be routed to
                     serviceName: route1-service
                     servicePort: 80
                 - path: /route2
                   backend:
                     serviceName: route2-service
                     servicePort: 80
             - host: domain2.com
               http:
                 paths:
                 - path: /route1 # /route1 will be replaced with /
                   backend: # where traffic will be routed to
                     serviceName: route1-service
                     servicePort: 80


        .. code-block:: sh

           kubectl apply -f Ingress-wear.yml
           kubect get ingress
           kubectl describe ingress ingress-wear
   
           # Imperative method
           kubectl create ingress ingree-wear --rule="domain1.com/route1=route1-service:80"


    * supported features differ based on the Ingress controller implementation
    * most features are exposed via annotations which apply to the entire Ingress object
    * can split single Ingress object into multiple to have different annotation scopes
    * Ingress controller should read multiple Ingress objects and merge together
    * will have undefined behavior for duplicate and conflicting configurations, and single
    implementation may behave differently
    * Ingress object can only refer to an upstream service in the same namespace, and can't use
    to point a subpath to a service in another
    * multiple objects in different namespaces can specify subpaths for the same host but
    Ingress must be coordinated globally across the cluster
    * no restrictions about which namespaces are allowed to specify which hostnames and paths
    * using TLS and HTTPS
        - need to specify a ``Secret`` with certificate and keys
        - undefined behavior if multiple objects specify certificates for the same hostname
        - can use API-driven [Let's Encrypt](https://letsencrypt.org/) and [cer-manager](https://cert-manager.io/)
    * implementations
        - each provider has cloud-based L7 load balancer Ingress implementation that take
          Ingress objects and configure via API
        - reduces the cluster and management load for the operators
        - GCE, Nginx, Contour, HAProxy, Traefik, Istio
        - NGINX Ingress controller has many features exposed via annotations
        - Envoy-based Emissary and Gloo focus on being API gateways, which includes resources
          for controlling Layer 4 balancing


mTLS
----
    * pod to pod encryption
    * instead of application level encryption, use third-party tools such as Istio, Linkerd,
    that uses mTLS
    * the tools provide service to service communication without depending on applications
    * capable of more than encryption, all related to connecting services in microservice
    architecture, known as service mesh
    * Istio insert a sidecar container into each pod
    * whenever a pod needs to send a message, Istio sidecar intercepts it and encrypts it
    * Istio sidecar on the receiver decrypts the message and passes to the target container
    * can configure Istio to use mTLS only if available
    * Permissive/Opportunistic mode: does not use mTLS if the external app does not support,
    sends the message only in the form the external app can interpret
    * Enforced/Strict mode: only mTLS is allowed, secure but can cause application to break

`back to top <#kubernetes>`_

Security
========

* `Authentication`_, `kubeconfig`_, `Authorization`_, `TLS in K8s`_, `Certificates API`_, `Image Security`_
* `K8s Attack Surface`_, `Kubelet Security`_, `Pod Security Policies`_, `OPA in K8s`_
* can configure security at container or pod level
    * pod level security configuration is carried to all containers in it
    * configuration at container level will override that of the pod level

    .. code-block:: yaml

       # security.yml
       apiVersion: v1
       kind: Pod
       metadata:
         name: myapp-pod
       spec:
         # pod level
         securityContext:
           runAsUser: 1000
         containers:
         - name: nginx
           image: nginx
           # container level
           securityContext:
             runAsUser: 1001
             capabilities:
               add: ["MAC_ADMIN"]


    .. code-block:: sh

       # to view user
       kubectl exec myapp-pod -- whoami



Authentication
--------------
    * kube-apiserver can authenticate using username, password, token, certificate, LDAP and
    service account
    * different authorizations such as RBAC, ABAC, Node, Webhook Mode
    * communication with other components is secured using TLS certificates
    * all user access is managed by kube-apiserver
    * all requests, from kubectl or direct access to api, goes through kube-apiserver
    * using csv file
        - a csv file with ``password,user,userID`` columns
        - pass it to authenticate using ``--basic-auth-file=users.csv``
        - ``kube-apiserver.service`` needs to be restarted
        - can have optional fourth column ``group``
        - can use tokens instead of passwords, use ``--token-auth-file``
    * using kubeadm
        - the kube-apiserver pod will be restarted once the ``kube-apiserver.yml`` file is
          modified
    * using curl
        - ``curl -v -k https://main-node-ip:6443/api/v1/pods -u "user1:password``
    * using static file is deprecated

kubeconfig
----------
    * stores how to find and authenticate to cluster
    * to authenticate using a file instead of long command
    * by default, kubectl look for config under ``$HOME/.kube/config``
    * ``kubectl get pods --kubeconfig config``, no need to specify if config file exists
    * the file has three sections: clusters, contexts, users
    * clusters
        - clusters that will be accessed
    * users
        - user accounts which have access to the clusters
    * contexts
        - define which user account will be used to access a cluster
    * example file

        .. code-block:: yaml

           apiVersion: v1
           kind: Config
           current-context: admin@my-cluster # kubectl will use this context
           clusters:
           - name: my-cluster
             cluster:
             certificate-authority: ca.crt
             certificate-authority: CERT_DATA # not using a file
             server: https://my-cluster:6443
           contexts:
           - name: admin@my-cluster
             context:
               cluster: my-cluster
               user: admin
               namespace: ns1
           users
           - name: admin
             user:
             client-certificate: admin.crt
             client-key: admin.key


    * doesn't need to create any object, file will be automatically used by kubectl

    .. code-block:: sh

       kubectl config view
       kubectl config view --kubeconfig=my-custom-config
   
       # change context to use user2 to access cluster1
       kubectl config use-context user2@cluster1



Authorization
-------------
    * authorization modes are set using ``--authorization-mode`` on kube-apiserver
    * set to ``AlwaysAllow`` by default
    * can provide multiple modes and requests are authorized in order specified, forwarded to
    the next if the first one denies
    * **AlwaysAllow** allows all without checking
    * **AlwaysDeny** denies all requests
    * **Node**
        - handles requests from users and kubelet
        - authorizes requests from users with the name 'system:node' and part of the 'system:node'
    * **ABAC**
        - Attribute-Based Access Control
        - associates users with permissions
        - permissions are created using json file passed to the kube-apiserver
        - must manually edit the file and restart kube-apiserver if permissions change
        - difficult to manage
    * **Webhook**
        - manage authorizations using third-party tools
        - e.g Open Policy Agent
    * **RBAC**
        - Role-Based Access Control
        - create a role with permissions and associate it to the users
        - only need to modify the role instead of modifying user
        - provide more standard approach

        .. code-block:: yaml

           # developer-role.yml
           apiVersion: rbac.authorization.k8s.io/v1
           kind: Role
           metadata:
             name: developer
           rules:
           - apiGroups: [""]
             resources: ["pods"]
             verbs: ["list", "get", "create"]
             resouceNames: ["web-app"] # only allow to access web-app pods
           - apiGroups: [""]
             resources: ["ConfigMap"]
             verbs: ["list", "get", "create"]
   
           # devuser-developer-role-binding.yml
           apiVersion: rbac.authorization.k8s.io/v1
           kind: RoleBinding
           metadata:
             name: devuser-developer-binding
           subjects: # user details
           - kind: User
             name: dev-user
             apiGroup: rbac.authorization.k8s.io
           roleRef: # created role details
             kind: Role
             name: developer
             apiGroup: rbac.authorization.k8s.io


        .. code-block:: sh

           kubectl apply -f developer-role.yml
           kubectl apply -f devuser-developer-role-binding.yml
           kubectl get roles
           kubectl get rolebindings
           kubectl describe roles developer
   
           # Imperative method
           kubectl create role developer --namespace=default --verb=list,get,create --resource=pods
           kubectl create rolebinding dev-user-binding --namespace=default --role=developer --user=dev-user


    * can check current access

        .. code-block:: sh

           # yes if allowed
           kubectl auth can-i create deployments
   
           # no if not allowed
           kubectl auth can-i delete nodes
   
           kubectl auth can-i delete nodes --as dev-user
           kubectl auth can-i delete nodes --as dev-user --namespace test


    * ``roles`` and ``rolebindings`` are for authorizing users to namespaced resources
    * Namespace scoped
        - resources that can specify namespace
        - pods, replicasets, jobs, deployments, services, sercrets, roles, rolebindings,
          configmaps, PVC
    * Cluster scoped
        - resources that don't need to specify namespace
        - such as nodes, PV, clusterroles, clusterrolebindings, certificatesigningrequests,
          namespaces

    .. code-block:: sh

       # list namespace scoped resources
       kubectl api-resources --namespaced=true
   
       # list non-namespace scoped resources
       kubectl api-resources --namespaced=false


    * to authorize users for cluster scoped, use ``clusterroles`` and ``clusterrolesbindings``
        - but can also be used to create namespaced resources

        .. code-block:: yaml

           # cluster-admin-role.yml
           apiVersion: rbac.authorization.k8s.io/v1
           kind: ClusterRole
           metadata:
             name: cluster-admin
           rules:
           - apiGroups: [""]
             resources: ["nodes"]
             verbs: ["list", "get", "create", "delete"]
   
           # cluster-admin-role-binding.yml
           apiVersion: rbac.authorization.k8s.io/v1
           kind: ClusterRoleBinding
           metadata:
             name: cluster-admin-binding
           subjects:
           - kind: User
             name: cluster-admin
             apiGroup: rbac.authorization.k8s.io
           roleRef:
             kind: ClusterRole
             name: cluster-admin
             apiGroup: rbac.authorization.k8s.io


        .. code-block:: sh

           kubectl apply -f cluster-admin.yml
           kubectl apply -f cluster-admin-role-binding.yml



TLS in K8s
----------
    * communication between all components in K8s need to be secured
    * all servers within the cluster must use server certificates and all clients must use
    client certificates
    * keys can be generated tools such as easyrsa, openssl, cfssl
    * CA certificates
        - K8s requires to have at least one CA for the cluster
        - the CA have its own ``ca.crt`` and ``ca.key``

        .. code-block:: sh

           # CA
           openssl genrsa -out ca.key 2048
           openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr
           openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt


    * client certificates
          1\. admin user requires ``admin.crt`` and ``admin.key`` to communicate with kube-apiserver
                    2\. kube-scheduler requires ``scheduler.crt`` and ``scheduler.key``
                    3\. kube-controller-manager requires ``controller-manager.crt`` and ``controller-manager.key``
                    4\. kube-proxy requires ``kube-proxy.crt`` and ``kube-proxy.key``
                    5\. kube-apiserver is also a client from etcdserver point of view, as it is the only
          component that communicates to it, kube-apiserver can use the keys generated or
          generate ``apiserver-etcd-client.crt`` and ``apiserver-etcd-client.key``
                    6\. kube-apiserver is also a client from kubelet server point of view, as it
          communicates to it, kube-apiserver can use the keys generated or
          generate ``apiserver-kubelet-client.crt`` and ``apiserver-kubelet-client.key``
                    7\. kubelet-server is also a client from kube-apiserver point of view, as it
          communicates to it, kubelet-server can use the keys generated or
          generate ``kubelet-client.crt`` and ``kubelet-client.key``
        - kube-apiserver needs to know which node is authenticating
        - certificates are named ``system:node:NODE_NAME``
        - group names must be added, ``system:nodes``, for API server to give right permissions

        .. code-block:: sh

           # Admin
           # admin must add group details in the certificate to differentiate from other users
           # "system:masters" group, with admin privileges, exists on K8s
           # use same process to generate all client certificates
           openssl genrsa -out admin.key 2048
           openssl req -new -key admin.key -subj "/CN=kube-admin/O=system:masters" -out admin.csr
           openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -out admin.crt
   
           # kube-scheduler, kube-controller-manager, kube-proxy
           # names must be prefixed with the keyword "system" as they are a system component part
           # of K8s control-plane


    * server certificates
          1\. kube-apiserver exposes https service, so ``apiserver.crt`` and ``apiserver.key`` must be
          generated
        - kube-apiserver is also called kubernetes, kubernetes.default, kubernetes.default.svc
          kubernetes.default.svc.cluster.local or IP address of the host running kube-apiserver
          or that of the pod running it
        - all the names must be present in certificate generated
        - ``--etcd-cafile``, ``--etcd-certfile``, ``--etcd-keyfile``, ``--client-ca-file``,
          ``--tls-cert-file``, ``--tls-private-key-file``, ``--kubelet-certificate-authority``,
          ``--kubelet-client-certificate``, ``--kubelet-client-key``
                    2\. etcd server also requires ``etcdserver.crt`` and ``etcdserver.key``
        - since etcdserver can be deployed as cluster across multiple servers,
          additional peer certificates, ``etcdpeer1.crt`` and ``etcdpeer1.key``, must be
          generated to secure communication between each other
        - require CA root certificate to verify clients are valid
        - specify them while starting the server, ``--key-file``, ``--cert-file``,
          ``--peer-cert-file``, ``--peer-client-cert``, ``--peer-key-file``,
          ``--peer-trusted-ca-file``, ``--trusted-ca-file``
                    3\. kubelet server also requires ``kubelet.crt`` and ``kubelet.key``
        - need certificates for each in the node of the cluster
        - certificates will be named after nodes
        - must use the certificates in definition file for each node

        .. code-block:: sh

           # etcdserver
           # use same methods to generate
   
           # kube-apiserver
           openssl genrsa -out apiserver.key 2048
   
           # must create to openssl.cnf specify all alternate names
           # openssl.cnf
           # [req]
           # req_extensions = v3_req
           # distinguished_name = req_distinguished_name
           # [ v3_req ]
           # basicConstraints = CA:FALSE
           # keyUsage = nonRepudiation,
           # subjectAltName = @alt_names
           # [alt_names]
           # DNS.1 = kubernetes
           # DNS.2 = kubernetes.default
           # DNS.3 = kubernetes.default.svc
           # DNS.4 = kubernetes.default.svc.cluster.local
           # DNS.5 = HOST_IP
           # DNS.6 = POD_IP
   
           openssl req -new -key apiserver.key -subj "/CN=kube-apiserver" \
           -out apiserver.csr -config openssl.cnf
           openssl x509 -req -in apiserver.csr -CA ca.crt -CAkey ca.key -CAcreateserial \
           -out apiserver.crt -extensions v3_req -extfile openssl.cnf -days 1000


        .. code-block:: yaml

           # kubelet-config-node01.yaml
           kind: KubeletConfiguration
           apiVersion: kubelet.config.k8s.io/v1beta1
           authentication:
             x509:
               clientCAFile: "/var/lib/kubernetes/ca.pem"
           authorization:
             mode: Webhook
           clusterDomain: "cluster.local"
           clusterDNS:
           - "10.32.0.10"
           podCIDR: "${POD_CIDR}"
           resolvConf: "/run/systemd/resolve/resolv.conf"
           runtimeRequestTimeout: "15m"
           tlsCertFile: "/var/lib/kubelet/kubelet-node01.crt"
           tlsPrivateKeyFile: "/var/lib/kubelet/kubelet-node01.key"


    * generated certificates can be used for API calls

        .. code-block:: sh

           curl https://kube-apiserver:6443/api/v1/pods \
           --key admin.key --cert admin.crt --cacert ca.crt


    * or move all the parameters into kube-config

        .. code-block:: yaml

           # kube-config.yml
           apiVersion: v1
           clusters:
           - clusters:
               certificate-authority: ca.crt
               server: https://kube-apiserver:6443
             name: kubernetes
           kind: Config
           users:
           - name: kubernetes-admin
             user:
               client-certificate: admin.crt
               client-key: admin.key



Certificates API
----------------
    * CA files for K8s are stored in CA server
    * can only sign a certificate by logging in into the CA server
    * since the files are stored in the control-plane node, it is the CA server
    * can send CSR directly to K8s through Certificates API call, no need to login to the server
    * admin can create CertificateSigningRequest object
    * once the object is created, all CSR can be seen by admins, review, approve and share
    with the users
    * all certificate related operations are carried out by the kube-controller-manager
    * ``signerName: kubernetes.io/kube-apiserver-client``, for client authentication

    .. code-block:: yaml

       # user1-csr.yml
       apiVersion: certificates.k8s.io/v1
       kind: CertificateSigningRequest
       metadata:
         name: user1
       spec:
         groups:
         - system:authenticated
         usages:
         - digital signature
         - key encipherment
         - server auth
         request:
           userCSRinEncodedForm


    .. code-block:: sh

       kubectl apply -f user1-csr.yml
       kubectl get csr
       kubectl certificate approve user1
       kubectl get csr user1 -o yaml


    * **Viewing Certificate Details**
        - look for ``/etc/kubernetes/manifests/kube-apiserver.yml`` file
        - ``command:`` under ``containers:`` has all the information
        - after decoding the certificate, start by looking ``subject:``, which has certificate
          name, then look for ``X509v3 Subject Alternative Name``, ``Not After``, ``Issuer``
        - when built from scratch, view logs with ``journalctl -u etcd.service -l``
        - in kubeadm, ``kubectl logs etcd-controlplane``
        - if the core components are down, ``docker logs CONTAINER_ID``

        .. code-block:: sh

           # /etc/kubernetes/pki/apiserver.crt
           # decode and view details
           openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout



Image Security
--------------
    * ``image: nginx`` is equal to ``image: docker.io/library/nginx``
    * docker.io: registry
    * library: user/account
    * nginx: image/repository
    * can use private registries
    * to pass credentials for private registry, first create secret object

    .. code-block:: sh

       kubectl create secret docker-registry regcred \
       --docker-server=private-registry.io \
       --docker-username=registry-user \
       --docker-password=registry-password \
       --docker-email=registry-user@org.com


    .. code-block:: yaml

       # custom-image-pod.yml
       spec:
         containers:
         - name: app1
           image: private-registry.io/apps/app1
         imagePullSecrets:
         - name: regcred # secret object name



K8s Attack Surface
------------------
    * example attack
        - attacker tries to find the IP address of the application, ``ping www.site.com`` and
          the site responds with IP
        - then port scan the server and one port is opened, e.g docker port
        - run docker command with the hostname, ``docker -H www.site.com ps``, and success
        - run a new privileged container in the environment,
          ``docker -H www.site.com run --privileged -it ubuntu bash``
        - from the container, install the necessary utilities and download the attack script
        - runs the script to escape from the container onto the host
        - run various commands on the host to get more information about the infrastructure
        - find out about K8s dashboard, ``iptables -L -t nat | grep kubernetes-dashboard``
        - can visit the K8s dashboard as it is setup without security
        - information about the whole cluster is now exposed
    * 4C's of Cloud Native Security
          1\. Cloud: security of the entire infrastructure, e.g data center, network, servers
                    2\. Cluster: e.g authentication, authorization, admission, network policy
                    3\. Container: e.g restrict images, supply chain, sandboxing, privileged
                    4\. Code: code with security best practices
    * [Security Overview](https://kubernetes.io/docs/concepts/security/overview/)

Kubelet Security
----------------
    * kubelet must be installed manually if using kubeadm
    * most parameters for ``kubelet.service`` startup are moved into ``kubelet-config.yaml``
    * pass the file as argument ``--config=/var/lib/kubelet/kubelet-config.yaml``
    * kubeadm auto configure kubelet configuration file on each node when ``kubeadm join``
    * view kubelet options, ``ps -aux | grep kubelet``
    * serves on two ports
        - 10250: serves API that allows full access
        - 10255: serves API that allows unauthenticated read-only access
    * allows anonymous access to its API by default
        - ``curl -sk https://localhost:10250/pods``
        - ``curl -sk https://localhost:10255/metrics``
        - set ``--anonymous-auth=false``
    * **certificate authentication (x509)**
        - default approach by kubeadm
        - ``--client-ca-file=/path/to/ca.crt``
        - ``curl -sk https://localhost:10250/pods/ --key key.pem --cart cert.pem``
        - from kube-apiserver, ``--kubelet-client-certificate``, ``--kubelet-client-key``
    * also bearer token authentication is available
    * if both authentication methods explicitly reject a request, by default will allow
    anonymous request of user, `system-anonymous`, with group, `system-unauthenticated`
    * **authorization**
        - default mode, ``--authorization-mode=AlwaysAllow``
        - disable ``localhost:10255/metrics``, ``--read-only-port=0``, default is set to 10255
        - if kubelet config file is in use, ``readOnlyPort`` defaults to 0

    .. code-block:: yaml

       # kubelet-config.yml
       apiVersion: kubelet.config.k8s.io/v1
       kind: KubeletConfiguration
       clusterDomain: cluster.local
       fileCheckFrequency: 0s
       healthzPort: 10248
       clusterDNS:
       - 10.86.0.10
       httpCheckFrequency: 0s
       sysncFrequency: 0s
       # --read-only-port=0
       readOnlyPort: 0
       authentication:
         # --anonymous-auth
         anonymous:
           enabled: false
         # --client-ca-file
         x509:
           clientCAFile: /path/to/ca.crt
       authorization:
         # --authorization-mode=Webhook
         mode: Webhook


* always verify platform binaries before applying
    * ``shasum -a 512 kubernetes.tar.gz``
    * ``shasum512 kubernetes.tar.gz``

Pod Security Policies
---------------------
    * deprecated in v1.21, removed in v1.25, use [Pod Security Admission](https://kubernetes.io/docs/concepts/security/pod-security-admission/) or third-party
    admission plugin
    * restrict pods from being created with specific capabilities
    * one of admission controllers, ``PodSecurityPolicy``
    * not enabled by default, ``kube-apiserver -h | grep enable-admission-plugins``
    * observes all pod creation requests and validate against the policy
    * both enable the plugin and be authorized to policy API
    * create a role and bind it to a pod to be authorized to API

    .. code-block:: yaml

       # psp.yml
       apiVersion: policy/v1beta1
       kind: PodSecurityPolicy
       metadata:
         name: example-psp
       spec:
         # reject containers with true
         privileged: false
         seLinux:
           rule: RunAsAny
         supplementalGroups:
           rule: RunAsAny
         runAsUser:
           rule: RunAsAny
         fsGroup:
           rule: RunAsAny
   
       # psp-role.yml
       metadata:
         name: psp-role
       rules:
       - apiGroups: ["policy"]
         resources: ["podsecuritypolicies"]
         resroucesNames: ["example-psp"]
         verbs: ["use"]
   
       # psp-rolebinding.yml
       subjects:
       - kind: ServiceAccount
         name: default
         namespace: default
       roleRef:
         kind: Role
         name: psp-role
         apiGroup: rbac.authorization.k8s.io/v1



OPA in K8s
----------
    * can connect Webhook controllers to OPA instead of custom server
    * **kube-mgmt**
        - deployed as sidecar alongside OPA
        - used to replicate resource definitions from K8s to be cached in OPA
        - the cached info can be imported to be used in policies
        - also used to load policies into OPA by creating configmaps, no need to manually add
        - ``kube-mgmt`` auto identifies the policies in the configmap and load them into OPA
        - or ``kubectl create configmap policy-podname --from-file=kubernetes.rego``
    * deploy OPA and kube-mgmt in ``opa`` namespace with necessary roles and rolebindings and
    have a service for OPA

    .. code-block:: yaml

       # opa-validate-webhook.yml
       webhooks:
       - clientConfig:
           # since OPA is deployed in cluster as service
           caBundle: $(cat ca.crt | base64 | tr -d '\n')
           service:
             namespace: opa
             name: opa


    .. code-block:: js

       // kubernetes.rego
       package kubernetes.admission
       // get information about all pods in the cluster
       import data.kubernetes.pods
       deny[msg] {
           input.request.kind.kind == "Pod"
           image := input.request.object.spec.containers[_].image
           startswith(image, "hooli.com/")
           msg := sprintf("image '%v' from untrusted registry", [image])
       }


    .. code-block:: yaml

       # using kube-mgmt
       # kubernetes.rego
       kind: ConfigMap
       apiVersion: v1
       metadata:
         name: policy-podname
         namespace: opa
         labels:
           openpolicyagent.org/policy: rego
       data:
         main:
          # rego policy added here


`back to top <#kubernetes>`_

Configuration
=============

* `Declarative`_, `Imperative`_, `Namespaces`_, `Labels`_, `Annotations`_
* `Specify arguments and commands`_, `ConfigMaps`_, `Secrets`_
* `Setting ENV variables`_, `Resource Requirements`_, `Service Accounts`_, `Taints & Tolerations`_
* `Node Selectors`_, `Node Affinity`_, `Custom Resource Definition`_

Declarative
-----------
    * everything in K8s is a declarative configuration object that represents the desired state
    * create set of definition/manifest files
    * storing declarative configuration in source control is often referred to as IaC

    .. code-block:: sh

       # creating, deleting, updating object
       # will create if doesn't exist
       # will look at the existing configuration and figure out what changes need to be made
       # never throws an error
       kubectl apply -f definition.yml



Imperative
----------
    * state is defined by the execution of a series of instructions
    * use imperative commands to do one time tasks quickly
    * ``--dry-run``: resource will be created as soon as the command is run
    * ``--dry-run=client``: just test the command without creating the resource
    * ``-o yaml``: output the resource definition in YAML format on screen
    * use ``kubectl expose`` for most commands to auto set selectors
    * generate a definition file if need to specify a node port

    .. code-block:: sh

       # generate deployment with 4 replicas
       kubectl create deployment nginx --image=nginx --replicas=4
   
       # scale a deployment
       kubectl scale deployment nginx --replicas=4
   
       # create pod with exposed port 80 and service of the same name
       kubectl run httpd --image=httpd --port=80 --expose=true
   
       # generate YAML files
       kubectl run nginx --image=nginx --dry-run=client -o yaml
   
       kubectl create deployment --image=nginx nginx --dry-run -o yaml
   
       # create YAML file and edit replicas to scale
       kubectl create deployment nginx --image=nginx --dry-run=client \
       -o yaml > nginx-deployment.yaml
   
       # service type of ClusterIP and expose pod on port 6379, use pod's label as selector
       # cannot pass in selectors as option
       kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml
   
       # will not use pod's label as selectors, assume selector as 'app=redis'
       kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml
   
       # service of type NodePort, use pod's labels as selectors, cannot specify the node port
       kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml
   
       # will not use pods labels as selectors
       kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
   
       ###############
       # create objects
       kubectl run --image=nginx nginx
       kubectl create deployment --image=nginx nginx
       kubectl expose deployment nginx --port 80
   
       # object cannot exist
       kubectl apply -f nginx.yml
   
       # update objects
       kubectl edit deployment nginx
       kubectl scale deployment nginx --replicas=5
       kubectl set image deployment nginx nginx=nginx:1.18
   
       # after editing local definition file, update the object and changes made are recorded
       kubectl replace -f nginx.yml
   
       # delete and recreate, object needs to exists first
       kubectl replace --force -f nginx.yml


Namespaces
----------
    * isolate resources, each namespace has policies and quota of resources
    * **default**
        - everything is done in ``default`` namespace
        - created at when cluster is setup
        - isolate users from interacting with internal resources
    * **kube-system**
        - for internal resources required by K8s
    * **kube-public**
        - for resources that are available for all users
    * can have ``namespace`` under ``metadata`` section in pod-definition.yml to set the pod be always
    created in `dev`

    .. code-block:: sh

       # list the pods in other namespace
       kubectl get pods --namespace=kube-system
   
       #list pods in all namespaces
       kubectl get pods --all-namespaces
   
       # create the pod in `dev` namespace
       kubectl apply -f pod-definition.yml --namespace=dev
   
       # create new namespace `dev`
       kubectl create namespace dev
   
       # get all namespaces
       kubectl get namespaces
   
       # run nginx pod in `dev` namespace
       kubectl run nginx --image=nginx -n=dev


    * create new namespace using YAML

        .. code-block:: yaml

           # namespace-dev.yml
           apiVersion: v1
           kind: Namespace
           metadata:
             name: dev


    * **Switching namespace**
        - ``kubectl config set-context $(kubectl config current-context) --namespace=dev``
    * **Resource Quota**
        - to limit resources in a namespace

        .. code-block:: yaml

           # compute-quota.yml
           apiVersion: v1
           kind: ResourceQuota
           metadata:
             name: compute-quota
             namespace: dev
           spec:
             hard:
               pods: "10"
               requests.cpu: "4"
               requests.memory: 5Gi
               limits.cpu: "10"
               limits.memory: 10Gi



Labels
------
    * key/value pairs for identifying metadata for objects
    * used for grouping, viewing and operating of the object
    * key
        - optional prefix and name, separated by a slash
        - prefix: must be a DNS subdomain with 253 characters max
        - name: required, 63 characters max
    * value
        - strings with 63 characters max
    * e.g ``kubernetes.io/cluster-service: "true"``
    * ``kubectl label`` command only changes the label on the deployment itself and won't affect
    any objects it creates
    * label selectors are used to filter objects
    * ``pod-template-hash`` label is applied by the deployment to track which pods were generated
    from which template versions

    .. code-block:: sh

       kubectl run myapp-pod --labels="type=web,env=prod"
   
       # show labels
       kubectl get pods --show-labels
   
       # edit labels
       kubectl label pods myapp-pod "env=test"
   
       # remove labels
       kubectl label deployments myapp "env-"
   
       # list pods that has specific label
       kubectl get pods --selector="type=web"
       # NOT
       kubectl get pods --selector="type!=web"
       # AND
       kubectl get pods --selector="type=web,env=test"
       # one of the values
       kubectl get pods --selector="env in (test,prod)"
       # not of the values
       kubectl get pods --selector="env notin (test,prod)"
       # key is set
       kubectl get pods --selector="env"
       # key is not set
       kubectl get pods --selector="!env"
       # combinations
       kubectl get pods -l "type=web,!env"


    * most objects support newer set of selector operators that can be used in YAML

        .. code-block:: yaml

           selector:
             matchLabels:
               type: web
             matchExpressions:
             - {key: env, operator: In, values: [test, prod]} # evaluated as AND
   
           # older form
           selector:
             type: web
             env: prod



Annotations
-----------
    * not meant for querying, filtering or differentiating pods from each other
    * a place to store additional metadata only to assist tools and libraries
    * can be used for the tool itself or pass information between external systems
    * add information as annotation and change to label if it will be used as selector
    * use cases
        - track/detect change cause, communicate policy to specialized scheduler
        - attach build, release, or image information
        - provide extra data for a UI
        - encode parameters for alpha functionality
        - enable Deployment object to keep track of ReplicaSet for rollout status and provide
          necessary data to roll back
    * good for small bits of data that are associated with specific resource
    * key
        - 'namespace' part is more important as they are used to communicate information
          between tools
    * value
        - free-form string field to store arbitrary data of any format
* when domain names are used in labels and annotations, they are expected to be aligned to that
particular entity

Specify arguments and commands
------------------------------

    .. code-block:: yaml

       # pod-definition.yml
       apiVersion: v1
       kind: Pod
       metadata:
         name: ubuntu-sleeper-pod
       spec:
         containers:
         - name: nginx-container
           image: nginx
           command:
             - "sleep"
             - "100"


    * ``args`` overrides CMD, ``command`` overrides ENTRYPOINT in Dockerfile

    .. code-block:: yaml

       # pod-definition.yml
       apiVersion: v1
       kind: Pod
       metadata:
         name: ubuntu-sleeper-pod
       spec:
         containers:
         - name: nginx-container
           image: nginx
           command: ["sleep2.0"]
           args: ["10"]


    .. code-block:: sh

       # create pod with custom args
       kubectl run nginx --image=nginx -- CUSTOM_ARGUMENT
   
       # create pod with custom args and commands
       kubectl run nginx --image=nginx --command -- COMMAND ARGUMENT



ConfigMaps
----------
    * separate env variables from definition file
    * in key/value pairs
    * name appropriately as it can be associated with pods

    .. code-block:: yaml

       # config-map.yml
       apiVersion: v1
       kind: ConfigMap
       metadata:
         name: app-config
       data:
         ENV_NAME1: ENV_VALUE1
         ENV_NAME2: ENV_VALUE2


    .. code-block:: sh

       ## Imperative method
       # '--from-literal' specifies key/value pair, specify '--from-literal' multiple times
       kubectl create configmap app-config --from-literal=ENV_NAME=ENV_VALUE
   
       # get from a file
       kubectl create cm app-config --from-file=app_config.properties
   
   
       ## Declarative method
       kubectl apply -f config-map.yml
   
       # configmaps info
       kubectl get configmaps
       kubectl describe configmaps



Secrets
-------
    * to store env variables in encrypted format, in key/value pairs
    * do not check secret definition files in SCM
    * [enable encryption at rest](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/) to be stored encrypted in etcd
    * a secret is only send to a node if a pod on that node requires it
    * kubelet stores the secret into a tmpfs
    * kubelet deletes local copy of secret if the pod is deleted
    * can use tools like Helm Secrets, HashiCorp Vault

    .. code-block:: yaml

       # secret-data.yml
       apiVersion: v1
       kind: Secret
       metadata:
         name: app-secret
       data:
         DB_HOST: iNenCodeDforMaT


    .. code-block:: sh

       ## Imperative method
   
       kubectl create secret generic app-secret --from-literal=DB_HOST=mysql
   
       # get from a file
       kubectl create secret app-secret --from-file=app_secret.properties
   
       # secrets must be encoded first to store in YAML file
       echo -n 'mysql' | base64
       # decode sercrets
       echo -n 'iNenCodeDforMaT' | base64 --decode
   
       ## Declarative method
       kubectl apply -f secret-data.yml
   
       # secrets info
       kubectl get secrets
   
       # hides the value, only show size
       kubectl describe secrets
   
       # show in encoded format
       kubectl get secrets app-secret -o yaml



Setting ENV variables
---------------------

    .. code-block:: sh

       kubectl run nginx --image=nginx --env="ENV_NAME1=ENV_VALUE1"


    * plain key/value format

        .. code-block:: yaml

           # pod-definition.yml
           apiVersion: v1
           kind: Pod
           metadata:
             name: ubuntu-sleeper-pod
           spec:
             containers:
             - name: nginx-container
               image: nginx
               ports:
               - conatinerPort: 8080
               env:
               - name: ENV_NAME
                 value: ENV_VALUE



    * using configmaps and secrets

        .. code-block:: yaml

           # pod-definition.yml
           spec:
             containers:
               - image: my-container
                 name: my-container
                 envFrom:
                 - configMapRef:
                     name: app-config
                 # OR
                 - secretKeyRef:
                     name: app-secret
                     key: DB_PASSWORD


    * injecting single env to a pod

        .. code-block:: yaml

           # pod-definition.yml
           spec:
             containers:
               - image: my-container
                 name: my-container
                 env:
                 - name: ENV_NAME1
                   valueFrom:
                     configMapKeyRef:
                       name: app-config
                       key: ENV_NAME1
                 # OR
                 - name: DB_PASSWORD
                   valueFrom:
                     secretKeyRef:
                       name: app-secre
                       key: DB_PASSWORD


    * injecting as files in a volume to a pod
        - each attribute in a secret is created as a file with the value as content

        .. code-block:: yaml

           # pod-definition.yml
           spec:
             containers:
               - image: my-container
                 name: my-container
                 volumes:
                 - name: app-config-volume
                   configMap:
                     name: app-config
                 # OR
                 - name: app-secret-volume
                   secret:
                     secretName: app-secret


* efficiency is measure in utilization, the amount of resource actively being used divided by
the amount of resource that has been purchased

Resource Requirements
---------------------
    * scheduler move pods to another node if the current is insufficient of resources
    * scheduler holds the pod in pending state if all nodes are out of resources
    * ``requests``: minimum amount of resource required to run the application
    * ``limits``: maximum amount of resource that an application can consume
    * can use ``MB/GB/PB`` or ``MiB/GiB/PiB``
    * resources are requested per container, not per pod
    * K8s limits 1 vCPU, 512 Mi per pod by default
    * K8s throttles the pod if cpu limit is reached and the pod is terminated if memory limit
    is reached
    * scheduler will ensure the sum of all requests of all pods on a node does not exceed the
    capacity of the node
    * if a container is over its memory request, the OS can't remove memory from the process as
    it has been allocated
    * when the system runs out of memory, ``kubelet`` terminates containers whose memory usage is
    greater than their requested memory and containers are auto restarted but with less
    available memory
    * can modify resource values in definition file

    .. code-block:: yaml

       # pod-definition.yml
       spec:
         containers:
         - resources:
             requests:
               memory: "1Gi"
               cpu: 1
             limits:
               memory: "2Gi"
               cpu: 2


    * ``cpu: 1`` equals 1 AWS vCPU, 1 GCP Core, 1 Azure Core, 1 Hyperthread

Service Accounts
----------------
    * used by applications to interact with K8s cluster
    * e.g used by Jenkins to deploy on the cluster
    * **token**
        - auto created when a service account is created
        - must be used by external applications to authenticate
        - stored as secret object which is linked to the service account
        - token can be mounted as volume to the pod if the third-party application is also
          hosted on K8s cluster

    .. code-block:: sh

       kubectl create serviceaccount my-svc-account
       kubectl get serviceaccounts
       kubectl describe sa my-svc-account
       kubectl describe secret my-svc-account-token
       kubectl get roles
       kubectl get rolesbinding


    * each namespace has its own default service account
    * when a pod is created, the default service account and the token are auto mounted
    * default service account can only run basic K8s API queries

    .. code-block:: yaml

       # pod-definition.yml
       spec:
         serviceAccountName: my-svc-account
         # OR
         automountServiceAccountToken: false



Taints & Tolerations
--------------------
    * to set restrictions on what pods can be scheduled on a node
    * pods do not have toleration by default
    * must specify which pods are tolerant to a taint
    * taints are set on nodes, tolerations are set on pods
    * taint effects
        - NoSchedule, PreferNoSchedule
        - NoExecute: new pods will not be scheduled, existing pods on the node will be evicted
          if they do not tolerate
    * taints and tolerations do not tell the pod to go to a particular node
    * a taint is set on control-plane node automatically to not accept any pods

    .. code-block:: sh

       # add taint
       kubectl taint nodes node-name key=value:taint-effect
       kubectl taint nodes node1 app=nginx:NoSchedule
   
       # remove taint
       kubectl taint nodes node1 app=nginx:NoSchedule-


    .. code-block:: yaml

       # pod-definition.yml
       spec:
         tolerations:
         - key: "app"
           operator: "Equal"
           value: "nginx"
           effect: "NoSchedule"



Node Selectors
--------------
    * limitation on a pod to only run on particular node

    .. code-block:: yaml

       # pod-definition.yml
       spec:
         nodeSelector:
           size: Large


    * ``size: Large`` is key/value pair assigned to a node, so the scheduler can match the label
    to match the pod on the right node
    * nodes must be labelled before a pod is created

    .. code-block:: sh

       # label a node
       kubectl label nodes node01 size=large


    * can only use exact label to achieve the requirement

Node Affinity
-------------
    * ensure pods run on particular nodes using advanced expressions

    .. code-block:: yaml

       # pod-definition.yml
       spec:
         affinity:
           nodeAffinity:
             requiredDuringSchedulingIgnoredDuringExecution:
               nodeSelectorTerms:
               - matchExpressions:
                 - key: size
                   operator: In
                   values:
                   - Large
                   - Medium


    * ``operator: In`` makes sure that the pod will be placed on a node whose label ``size`` has
    any value specified in `values:`
    * ``operator: NotIn`` set the pod on the node whose label ``size`` is not ``Large`` or ``Medium``
    * ``operator: Exists`` set the pod on the node if label ``size`` exists, does not compare values
    * node affinity types
        - ``requiredDuringSchedulingIgnoredDuringExecution``
        - ``preferredDuringSchedulingIgnoredDuringExecution``
        - ``requiredDuringSchedulingRequiredDuringExecution``
    * **DuringScheduling**
        - state where a pod does not exist and created for the first time
        - "Required": scheduler mandates that the pod be placed on a node with the given
          affinity rules, pod will not be scheduled if cannot match the label, used when the
          placement of the pod is important
        - "Preferred": if no matching node is found, scheduler ignores the affinity rules and
          place the pod on any available node
    * **DuringExecution**
        - state where a pod is running and a change is made that affect the node affinity
        - "Ignored": pod will still run and node affinity changes will not affect once they
          are scheduled
        - "Required": running pods will be evicted if they do not match
* using Taints/Tolerations and Node Affinity together give more control over nodes and pods

Custom Resource Definition
--------------------------
    * built-in resources have built-in controllers to handle them
    * CRD tells K8s objects of custom kind to be created
    * CRD only will store custom objects to be stored in etcd, needs a controller

    .. code-block:: yaml

       # custom-object.yml
       apiVersion: custom.com/v1
       kind: CustomObject
       metadata:
         name: my-custom-object
       spec:
         value: 2
         attr: hello
   
       # custom-definition.yml
       apiVersion: apiextensions.k8s.io/v1
       kind: CustomResourceDefinition
       metadata:
         name: test.custom.com
       spec:
         scope: Namespaced
         group: custom.com
         names:
           kind: CustomObject
           singular: customobject # to be output of kubectl
           plural: customobjects # use by API resource
           shortNames:
             - cobj
         versions:
         - name: v1
           served: true
           storage: true
         schema: # parameters under spec in object definition file
           openAPIV3Schema:
             type: object
             properties:
               spec:
                 type: object
                 properties:
                   value:
                     type: integer
                     minimum: 2
                     maximum: 10
                   attr:
                     type: string


    .. code-block:: sh

       # create CRD first
       kubectl apply -f custom-definition.yml
   
       # can now create custom object
       kubectl apply -f custom-object.yml


`back to top <#kubernetes>`_

Observability
=============

* `Health Checks`_, `Readiness Probes`_, `Liveness Probes`_, `Startup Probes`_, `Exec Probes`_
* `Logging`_, `Monitoring`_
* `Application Failure`_, `Controlplane Failure`_, `Node Failure`_, `Network Failure`_
* `Auditing`_, `K8s Dashboard`_, `JSON Path`_

Health Checks
-------------
    * health checks ensure that the main process of application is always running and will be
    restarted if it isn't
    * by default, K8s assumes the container is ready to serve user traffic as soon as it is
    created and so sets the condition of container's Ready status to true
    * sometimes the application might take time to load and the service is unaware of that the
    application is not ready
    * liveness health checks are application-specific and have to define in pod manifest
    * K8s also supports ``tcpSocket`` health checks which are useful for non-HTTP applications
    such as databases

Readiness Probes
----------------
    * to tie the Ready condition to the actual state of the application
    * e.g run http test to check if the API server responds, test to check if a TCP socket is
    listening in a database, execute a command in the container that exits if application is
    running successfully
    * when ``readinessProbe`` is configured, K8s does not immediately set the Ready condition to
    true when the container is created
    * instead it tests the API to check if it responds correctly
    * if the readiness checks fail, the container is removed from service load balancers
    * ``periodSeconds``: how often to probe
    * the probe will stop if the application is not ready after 3 attempts
    * ``failureThreshold``: to make more attempts

    .. code-block:: yaml

       # pod-definition.yml
       spec:
         containers:
         - name: webapp
           image: webapp
           ports:
           - containerPort: 8080
           readinessProbe:
             httpGet:
               path: /api/ready
               port: 8080
             # OR
             tcpSocket:
               port: 3306
             # OR
             exec:
               command:
               - echo
               - "hello"
             initialDelaySeconds: 10
             periodSeconds: 5
             failureThreshold: 8



Liveness Probes
---------------
    * to test if the application in the container is healthy
    * defined per container, each container inside a pod is health checked separately
    * HTTP status code must be equal to or greater than 200 and less than 400 to be cosidered
    successful

    .. code-block:: yaml

       # pod-definition.yml
       spec:
         containers:
         - livenessProbe:
             httpGet:
               path: /api/ready
               port: 8080
             # OR
             tcpSocket:
               port: 3306
             # OR
             exec:
               command:
               - echo
               - "hello"
             initialDelaySeconds: 10
             periodSeconds: 5
             failureThreshold: 8


* combining readiness and liveness probes helps ensure only healthy containers are in cluster

Startup Probes
--------------
    * run before any other probes when a pod is started
    * proceeds until it either times out or succeeds,after which the liveness probe takes over

Exec Probes
-----------
    * execute a script or program in the context of the container
    * if the script returns a zero exit code, the probe succeeds
    * useful for custom application validation logic that doesn't fit an HTTP call

Logging
-------
    * must specify container name explicitly in a multi-container pod

    .. code-block:: sh

       kubectl logs -f myapp-pod
   
       # log specific container running in a pod
       kubectl logs -f myapp-pod -c container1



Monitoring
----------
    * K8s doesn't have built-in full feature monitoring solution
    * third-party tools such as Metrics Server, Prometheus, Elastic Stack, Datadog, Dynatrace
    * cAdvisor
        - sub component of kubelet
        - retrieves performance metrics from pods and expose them through kubelet api
    * **Metrics Server**
        - slimmed-down version of Heapster, in-memory monitoring
        - retrieves metrics from each node and pod, aggregates and stores them
        - get metrics from cAdvisor

        .. code-block:: sh

           # for minikube
           minikube addons enable metrics-server
   
           # others
           kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
   
           # check metrics
           kubectl top nodes
           kubectl top pods



Application Failure
-------------------
    * check accessibility
        - draw a map or chart of the application depending on information of failure
        - check every object in the map until the root cause of issue is found
    * check front-end
        - use standard testings if accessible
        - use ``curl http://web-service-ip:node-port`` if web application
    * check services
        - check the service, ``kubectl describe service web-service``, to see if endpoints are
          reached
        - compare the selectors and labels on pod and service
    * check pod
        - make sure the pod is in a running state, ``kubectl get pod``
        - check if there are any restarts
        - check the events, ``kubectl describe pod web``
        - check the logs, ``kubectl logs web -f`` or ``kubectl logs web -f --previous``
    * check dependent service
    * check dependent applications

Controlplane Failure
--------------------
    * check node status
        - ``kubectl get nodes``
    * check control-plane components, if deployed as pods
        - ``kubectl get pods -n kube-system``
    * check control-plane components, if setup with kubeadm
        - ``service kube-apiserver status``
        - ``service kube-controller-manager status``
        - ``service kube-scheduler status``
        - on worker node, ``service kubelet status``
        - on worker node, ``service kube-proxy status``
    * check service logs
        - ``kubectl logs kube-apiserver-controlplane -n kube-system``
        - or ``journalctl -u kube-apiserver``
    * [Debug service issues](https://kubernetes.io/docs/tasks/debug/debug-application/debug-service/)

Node Failure
------------
    * check node status
        - ``kubectl get nodes``
        - ``kubectl describe node node01``
        - make sure Type ``Ready`` is set to ``True``
    * check for possible memory, cpu and disk space on the node
        - ``top``, ``df -h``
    * check kubelet status
        - ``service kubelet status``
        - ``journalctl -u kubelet``
    * check certificates
        - ``openssl x509 -in /var/lib/kubelet/node01.crt -text``

Network Failure
---------------
    * CoreDNS memory usage is affected by the number of pods and services in the cluster
    * other factors such as size of the filled DNS answer cache, rate of queries received (QPS)
    per CoreDNS instance
    * K8s resources for coreDNS
        - service account named ``coredns``
        - clusterroles named ``coredns`` and ``kube-dns``
        - clusterrolebindings named ``coredns`` and ``kube-dns``
        - deployment named ``coredns``
        - config map named ``coredns``
        - service named ``kube-dns``
    * port 53 is used for DNS resolution
    * if coredns pods are in pending state, check network plugin is installed or not
    * if coredns pods have ``CrashLoopBackOff`` or ``Error state``
        - check or add ``resolvConf`` in kubelet config yaml
        - disable local DNS cache on host nodes and restore ``/etc/resolv.conf``
        - edit the ``Corefile``, replacing forward ``./etc/resolv.conf`` with the IP address of
          upstream DNS
    * check kube-dns service endpoints
        - ``kubectl -n kube-system get ep kube-dns``
    * check kube-proxy pod in kube-system namespace
    * check kube-proxy logs
    * check configmap is correctly defined and config file for running kube-proxy binary is
    correct
    * check kube-config is defined in the config map
    * check kube-proxy inside the container
        - ``netstat -plan | grep kube-proxy``
    * [Debugging DNS Resolution](https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/)

Auditing
--------
    * K8s provides auditing events handled by kube-apiserver
    * stages: ``RequestReceived``, ``ResponseStarted``, ``ResponseComplete``, ``Panic``
    * each stage generates events that can be recorded by kube-apiserver, but not by default
    * make sure to record only specific events by making use of policy objects
    * ``level``: logs verbose level, ``None``, ``Metadata``, ``Request``, ``RequestResponse``
    * need to enable backends
        - log backend: store audit events to a file on control-plane node
        - webhook backend: writes to a remote webhook such as Falco
    * in ``/etc/kubernetes/manifests/kube-apiserver.yaml``, ``--audit-log-path`` and
    ``--audit-policy-file``
    * ``--audit-log-maxage``, maximum days to keep audit records
    * ``--audit-log-maxbackup``, ``--audit-log-maxsize=100`` in megabytes

    .. code-block:: yaml

       # audit-policy.yml
       apiVersion: audit.k8s.io/v1
       kind: Policy
       omitStages: ["RequestReceived"]
       rules:
       - namespaces: ["namespace1"] # not mandatory, all by default
         verbs: ["delete"] # not mandatory, every action by default
         resources:
         - groups: "" # default to core group
           resources: ["pods"]
           resourceName: ["webapp-pod"]
         level: RequestResponse



K8s Dashboard
-------------
    * one of K8s sub-project
    * graphical representation of the cluster, monitor activities and provision new resources
    * not deployed by default
    * ``kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml``
    * when deployed, a namespace, deployment and service called ``kubernetes-dashboard`` are
    created, and a configmap is also created
        - do not use ``--disable-filter`` as it can be vulnerable to XSRF attacks
    * service is set to ``ClusterIP``, only accessible within the cluster
    * using ``kubectl proxy`` to access from outside the cluster
        - ``https://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy``
        - cannot make the dashboard accessible across the team
    * can expose as a NodePort, exposing as LoadBalancer is not recommended
    * use auth proxy like OAuth
    * using token to login
        - must create a user and gives necessary permissions using roles and role bindings
        - retrieve token, ``kubectl describe secrete kubernetes-dashboard-token``
    * can also use kubeconfig file to login
    * [K8s Dashboard](https://github.com/kubernetes/dashboard), [Dashboard Documentation](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)
    * Dashboard Security: [YouTube](https://www.youtube.com/watch?v=od8TnIvuADg), [blog post](https://blog.heptio.com/on-securing-the-kubernetes-dashboard-16b09b1b7aca)

JSON Path
---------
    * makes filtering data across large data sets using complex queries easier
    * kube-apiserver responds in JSON format and kubectl convert it to human readable format
    * during the conversion, a lot of information is hidden to make readable
    * hierarchy same as in definition files
    * using JSON Path
        - identify kubectl commands
        - familiarize with JSON output, ``kubectl get pods -o json``
        - form JSON Path query, ``.items[0].spec.containers[0].image`` (kubectl auto add '$')
        - use the JSON Path query with kubectl command
          ``kubectl get pods -o=jsonpath='{ .items[0].spec.containers[0].image }'``
    * examples
        - ``kubectl get pods nginx -o=jsonpath={.status.podIP}`` or
          ``kubectl get pods nginx -o jsonpath --template={.status.podIP}`` or
          ``kubectl get pods nginx -o jsonpath={.status.podIP}``, get IP of a pod
        - ``kubectl get nodes -o=jsonpath='{.items[*].metadata.name}'``, returns the names of
          the nodes in the cluster
        - ``kubectl get nodes -o=jsonpath='{.items[*].status.nodeInfo.architecture}'``, returns
          hardware architecture of the nodes
        - ``kubectl get nodes -o=jsonpath='{.items[*].status.capacity.cpu}'``, returns cpu
          counts on the nodes
        - can combine queries,
          ``kubectl get nodes -o=jsonpath='{.items[*].metadata.name}{.items[*].status.capacity.cpu}'``,
          returns the names of the nodes and cpu counts
        - can add ``{"\n"}``, ``{"\t"}`` between queries for better output
    * using loop, ``range``
        - for each item in the node, print the node name, insert a tab, print cpu count, print
          new line character
        - ``'{range .items[*]} {.metadata.name}{"\t"}{.status.capacity.cpu}{"\n"} {end}'``
    * can print custom columns
        - ``kubectl get nodes -o=custom-columns=<COLUMN NAME>:<JSON PATH>``
        - can sometimes be easier approach compare to loop method
        - must exclude ``.items`` as the custom-columns assume the query is for each item
        - ``-o=custom-columns=NODE:.metadata.name,CPU:.status.capacity.cpu``
    * can sort the output
        - ``kubectl get nodes --sort-by=.metadata.name``

`back to top <#kubernetes>`_

Cluster Maintenance
===================

* `Draining the node`_, `K8s Versions`_, `K8s API`_, `Backup`_
* if a node is down, K8s gives a node 5 min to comeback up
* if the node comeback up, kubectl process starts and the pods become online
* but if a node is down for more than 5 min, the pods are terminated from that node
* if the pods are part of the replicaset, they are recreated on other nodes
* ``--pod-eviction-timeout``, time it waits for a pod to comeback online
* if the node comeback online after pod-eviction-timeout, it has no pods on it
* can make a quick upgrade if it is sure that the node down time will be less than 5 min

Draining the node
-----------------
    * a safer way, so that the workloads are terminated and recreated on
    other nodes
    * when the node is drain, it is cordon or marked as unschedulable until the restriction is removed
    specifically
    * when uncordon, the pods from other nodes do not auto move back, only newly created
    or recreated pods will be on it
    * when a node is cordon, the pods on it do not auto move to other nodes, only make
    sure that the new pods are not scheduled on the node
    * cannot drain if pods are not managed by ReplicationController, ReplicaSet, Job,
    DaemonSet or StatefulSet, as pods will have to be recreated by a controller

    .. code-block:: sh

       kubectl drain node-1
       # OR
       kubectl drain node-1 --ignore-daemonsets
       kubectl uncordon node-1
   
       # pods do not move
       kubectl cordon node-2



K8s Versions
------------
    * major.minor.patch, e.g v1.24.4
    * minor: every few months with new features
    * patch: more often with bug fixes
    * all bug fixes and improvements first go in alpha release
    * new features are well tested on beta versions
    * different components have their own versions
    * no other component can have higher version than kube-apiserver as it is the primary
    component others talk to
    * kube-controller-manager and kube-scheduler can be at one version lower than the API server
    * kubelet and kube-proxy can be at two versions lower than the API server
    * but kubectl can be higher, same or lower than the API server
    * can upgrade each component if required
    * K8s support only up to the recent three minor versions
    * upgrade one minor version at a time
    * upgrade process depends on how the cluster is setup
    * easy to upgrade on cloud and if using kubeadm
    * have to manually upgrade different components if setup from scratch
    * first upgrade the controlplane node, then the worker nodes
    * components in the controlplane go down while upgrading
        - does not impact applications on worker nodes
        - cannot use ``kubectl`` command as all management tools also go down
        - but if a pod fails, new pods will not be auto created
    * after the controlplane is upgraded, upgrade the worker nodes
          1\. upgrade all worker nodes at the same time
                    2\. upgrade one node at a time
                    3\. add new nodes with newer version to the cluster and remove the old ones, strategy
          easy to use if on cloud
    * **upgrading with kubeadm**
        - does not install or upgrade kubelet on the nodes
        - upgrade the kubeadm tool first
        - ``VERSION`` output from ``kubectl get nodes`` is the version kubelet is registered to
          the API server, not the version of the API server
        - upgrade kubelet on controlplane, if exists
        - move the workloads on worker node to another with ``kubectl drain node-1``, and
          upgrade the necessary components
        - do the same on other nodes, one after another

        .. code-block:: sh

           # on controlplane node
           kubectl drain controlplane --ignore-daemonsets
           kubeadm upgrade plan
           apt-get update
           apt-get upgrade -y --allow-change-held-packages kubeadm=1.24.5-00
           kubeadm upgrade plan
           kubeadm upgrade apply v1.24.5
           apt-get upgrade -y --allow-change-held-packages kubelet=1.24.5-00 kubectl=1.24.5-00
           systemctl restart kubelet
           kubectl uncordon controlplane
   
           # must drain from control-plane node, do the same on other nodes one by one
           kubectl drain node-1 --ignore-daemonsets
   
           apt-get update
           apt-get upgrade -y --allow-change-held-packages kubeadm=1.24.5-00
           kubeadm upgrade node
           apt-get upgrade -y --allow-change-held-packages kubelet=1.24.5-00 kubectl=1.24.5-00
           systemctl restart kubelet
   
           # from controlplane node
           kubectl uncordon node-1



K8s API
-------
    * the only component that interact directly with etcd
    * has multiple groups for different purposes
    * /metrics, /healthz, /version, /api, /apis, /logs
    * **/api**
        - core group, /v1
        - such as namespaces, pods, rc, events, endpoints, nodes, bindings, PV, PVC,
          configmaps, secrets, services
    * **/apis**
        - named group, more organized
        - new features will be placed in named group
        - /apps/v1/deployments (/replicasets, /statefulsets)
        - /networking.k8s.io/v1/networkpolicies
        - /certificates.k8s.io/v1/certificatesigningrequests
        - /extensions, /storage.k8s.io, /authentication.k8s.io
        - top level are API groups
        - each group has resources (/deployments, /replicasets)
        - each resource has actions or verbs (list, get, create)
    * ``curl http://localhost:6443`` to view groups and resources, which needs credentials
    * ``kubectl proxy`` will start a kubectl proxy, which doesn't need credentials
    * kubectl proxy is not same as kube proxy, which is used to proxy pods
    * can enable API groups by editing ``--runtime-config``
    * versions
        - major.minor.patch
        - v1alpha1: not enabled by default, have bugs, lack e2e tests, may be dropped later,
          for expert users
        - v1beta1: minor bugs, has e2e tests, for beta testers
        - v1: GA/Stable, conformance tests, highly reliable, for all users
        - objects with different versions can run at the same time
        - only one can be ``preferredVersion``, to which API commands will be run on (can
          check by viewing groups and resources with API URL)
        - only one can be ``storageVersion``, to be stored in etcd (can only check by
          querying etcd database)
    * **API Deprecation Policies**
          1\. API elements may only be removed by incrementing the version of the API group
        - e.g element in v1alpha1 can only be removed in v1alpha2, still present in v1alpha1
                    2\. API objects must be able to round-trip between API versions in a given release
          without information loss, with the exception of whole REST resources that do not exist
          in some versions
        - e.g objects converted from v1alpha1 to v1alpha2, then back to v1alpha1, should be
          the same as v1alpha1 version
                    3\. API version in a given track may not be deprecated until a new API version at
          least as stable is released
                    4a\. Other than the most recent API versions in each track, older API versions must
          be supported after their announced deprecation for a duration of no less than:
          GA: 12 months or 3 releases, Beta: 9 months or 3 releases, Alpha: 0 releases
                    4b\. The "preferred" API version and the "storage version" for a given group may not
          advance until after a release has been made that supports both the new version and the
          previous version
    * converting API versions with Kubectl Convert (a plugin)

        .. code-block:: sh

           # apiVersion: apps/v1beta1 in old.yml
           kubectl convert -f old.yml --output-version apps/v1
   
           # check resource API group
           kubectl explain pod
   
           # create pod object without assigning to a node
           curl -X POST https://k8s_IP/api/v1/namespaces/default/pods

    * ``--etcd-servers``, specify location of etcd server
    * kubeadm deploy kube-apiserver as pod in kube-system namespace, definition file in
    ``etc/kubernetes/manifests/kube-apiserver.yml``
    * can be searched as a process ``ps -aux | grep kube-apiserver`` on the controlplane node
    * in non kubeadm setup, ``/etc/systemd/system/kube-apiserver.service``

Backup
------
    * use source code repository to store resource definitions
    * query kube-apiserver to save resource configurations
        - better way to backup when in managed environment
        - ``kubectl get all --all-namespaces -o yaml > all-deploy-services.yml``
    * can use tools like Velero to backup the cluster
    * can backup etcd instead of saving all resource definitions
        - ``--data-dir``, directory where all configurations will be stored
        - etcd has built-in snapshot too, ``etcdctl snapshot save snapshot.db``
        - ``service kube-apiserver stop && etcd snapshot restore snapshot.db --data-dir /dir``,
          new data directory is created in /dir, then configure the etcd.service to use the new
          data directory
        - ``systemctl daemon-reload && service etcd restart && service kube-apiserver start``
        - always authenticate etcd commands, ``--endpoints, --cacert, --cert, --key``
        - when etcd restore, it initializes a new cluster config and configure the members
        - as new members of the new cluster
        - if etcd is running as a pod, ETCD is set up as a Stacked ETCD Topology where the
          distributed data storage cluster provided by etcd is stacked on top of the cluster
          formed by the nodes managed by kubeadm that run control plane components.

        .. code-block:: sh

           ETCDCTL_API=3 etcdctl --endpoints=https://[127.0.0.1]:2379 \
           --cacert=/etc/kubernetes/pki/etcd/ca.crt \
           --cert=/etc/kubernetes/pki/etcd/server.crt \
           --key=/etc/kubernetes/pki/etcd/server.key \
           snapshot save /opt/snapshot-pre-boot.db
           Snapshot saved at /opt/snapshot-pre-boot.db


    * backing up etcd cluster: [K8s docs](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster), [github](https://github.com/etcd-io/website/blob/main/content/en/docs/v3.5/op-guide/recovery.md), [YouTube](https://www.youtube.com/watch?v=qRPNuT080Hk)

`back to top <#kubernetes>`_

Microservices Architecture
==========================

* `Sidecar`_, `Adapter`_, `Ambassador`_

Sidecar pattern
---------------
    * e.g deploying a logging agent alongside the web server with the agent sending logs to the
    central logging server

Adapter pattern
---------------
    * e.g logging agent is deployed alongside the web server, and the different formatted
    logs are processed before being sent to the logging server

Ambassador pattern
------------------
    * e.g application communicates to different databases at different stages of
    development, it is important to make sure to modify the code to have correct connectivity
    depending on the environment, this process is handed to a separate container within the pod
    so that the application can refer to the correct database
* `example voting app on docker`_, `example voting app on K8s`_

`back to top <#kubernetes>`_

# Designing a Cluster

* `Purpose`_, `Environment`_, `Choosing Workloads`_, `Choosing Infrastructure`_, `High Availability`_
* `Mutable vs Immutable Infrastructure`_

Purpose
-------
    * education
        - Minikube, single node cluster with kubeadm, GCP, AWS
    * development & testing
        - multi-node with single control-plane and multiple workers
        - setup using kubeadm or GCP, AWS or AKS
    * hosting production applications
        - high availability multi-node cluster with multiple control-plane nodes
        - kubeadm, GCP or Kops on AWS
        - up to 500 nodes, up to 150,000 pods in the cluster
        - up to 300,000 total containers, up to 100 pods per node

Environment
-----------
    * cloud: GKE, Kops for AWS, AKS
    * on premises: kubeadm

Choosing Workloads
------------------
    * storage
        - SSD for high performance, network based storage for multiple concurrent connections
        - persistent shared volumes for shared access across pods
        - label nodes with specific disk types
        - use node selectors to assign applications to nodes with specific disk types
    * nodes
        - VM or physical machines
        - minimum of 4 node cluster
        - Linux x86_64 architecture
        - best practice is to not host workloads on control-plane nodes
        - may choose to separate etcd from control-plane node

Choosing Infrastructure
-----------------------
    * local machines
        - can setup natively on Linux, VM or WSL on Windows
        - minikube: deploy VMs, single node cluster
        - kubeadm: requries VMs to be ready, single/multi node cluster
    * turnkey solutions
        - provision, configure and maintain VMs and use scripts to deploy cluster
        - e.g [Kubernetes on AWS](https://aws.amazon.com/kubernetes/) using [kops](https://kops.sigs.k8s.io/) or [KubeOne](https://www.kubermatic.com/products/kubermatic-kubeone/)
        - K8s certified solutions such as OpenShift, Cloud Foundry Container Runtime, VMware
          Cloud PKS, Vagrant
    * hosted
        - managed solutions, K8s-as-a-Service
        - CSP provision and maintain VMs and install K8s
        - e.g [AWS EKS](https://aws.amazon.com/eks/), [GKE](https://cloud.google.com/kubernetes-engine), [AKS](https://azure.microsoft.com/en-us/services/kubernetes-service/), [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift/kubernetes-engine)

High Availability
-----------------
    * have redundancy across every components
    * having two control-plane nodes
        - running multiple instances of the same components
        - bot kube-apiserver are in active-active mode
        - have load balancer in front of the nodes using solutions such as Nginx, HA proxy
        - then point kubectl to the load balancer
        - two instances of each kube-controller-manager and kube-scheduler must run in
          active-standby mode, which is decided through leader election process
        - ``--leader-elect`` , ``--leader-elect-lease-duration``, ``--leader-elect-renew-deadline``,
          ``--leader-elect-retry-period`` options are available
        - stacked topology: etcd is part of the control-plane node, easy to setup and manage,
          but both etcd go down if one node goes down
        - external etcd topology: etcd is separated from control-plane node, hard to setup but
          less failure impact
    * etcd
        - when distributing across multiple servers, data can be read from multiple nodes
        - only one is responsible for processing the write
        - one node is elected as leader, and others become followers
        - leader makes sure other nodes have copy of the data if write request is sent to
          it
        - if writes are sent to the followers, they are internally forwarded to the leader
        - uses RAFT for leader election
        - a write is complete if it can be written to majority of the nodes, Quorum = n/2 + 1
        - recommended to have at least 3 or odd number (5, beyond 5 is unnecessary) of
          instances, as even number of
          nodes has the possibility of failing when network is segmented
    * RAFT
        - uses random timer for initiating requests
        - the first one to finish the request sends permission to others to be a leader
        - it sends notification to others that it will continue to be the leader
        - if the other nodes do not receive notification from the leader, new leader election
          process is initiated among them

Mutable vs Immutable Infrastructure
-----------------------------------
    * **Mutable**
        - in-place update: updating software version by version on each server
        - underlying infrastructure remains the same
        - but software and configurations of the servers change as part of the update
        - upgrade can fail on one or more of the servers because of unmet dependencies,
          network issues, file system full, different OS versions
        - configuration drift: not all servers able to update leaving the infrastructure in
          a complex state, each server behaving different
    * **Immutable**
        - deploy new servers with new software, and delete the old ones if the new ones succeed
        - cannot carry out in-place updates
    * although containers are to be immutable by default, can still carry in-place updates
    * copying files statically into the pod, or log in into the container and making changes
    * to prevent in-place updating, make sure file system cannot be written once the pod
    started
        - using only ``readOnlyRootFileSystem``, application might break if it has to write logs
          or cache data
        - use volume mounts
        - can still make changes to sudo file system if container is run in privileged mode
    * to make infrastructure immutable, use least privileged principle and pod security policy

        .. code-block:: yaml

           # pod-definition.yml
           spec:
             containers:
             - securityContext:
                 readOnlyRootFileSystem: true
               volumeMounts:
                 - name: cache-volume
                   mountPath: /var/cache/
             volumes:
             - name: cache-volume
               emptyDir: {}


`back to top <#kubernetes>`_

System Hardening
================

* `Limit access to nodes`_, `Seccomp in K8s`_, `AppArmor in K8s`_, `Capabilities`_
* `Minimize base image footprint`_, `Image Policy Webhook`_
* reducing the complexity of the systems reduce the attack surface
* use least privilege principle make sure every systems only have minimum privileges to carry
out their tasks

Limit access to nodes
---------------------
    * limit exposure of control-plane and the nodes to the Internet
    * some managed clusters do not allow access to control-plane, varies on CSP
    * some solutions use private networks, the cluster can then be accessed via VPN
    * enable authorized access within firewall if not able to setup VPN
        - allow restricted access to the nodes based on source IP range
    * configure who can access
        - admins need ssh access as they need to manage the nodes
        - developers might have access in development environment but not in production
        - end users should be restricted from having access
    * accounts
        - user account: individuals who need access to Linux system, e.g admins, developers
        - superuser account: root account with UID 0, has unrestricted access
        - system account: created during OS installation, used by services not run
          as superuser
        - service account: created when services are installed
        - ``id``, ``who``, ``last``
        - ``usermod -s /bin/nologin user1``, ``userdel user1``, ``deluser user1 group1``
    * access control files
        - ``/etc/passwd``, ``/etc/shadow``, ``/etc/group``
    * SSH
        - disable password authentication and use key pairs to authenticate
        - ``PermitRootLogin no``, ``PasswordAuthentication no`` in ``/etc/ssh/sshd_config``
* remove obsolete packages and services
    * install only required packages
    * ``systemctl list-units --type service``
* restrict network access and obsolete kernel modules
    * loading modules, ``modprobe module1``
    * list modules, ``lsmod``
    * blacklist modules on all the nodes
    * edit ``etc/modprobe.d/blacklist.conf``, file name doesn't matter
    * e.g add ``blacklist sctp`` in the file
* identify and fix open ports
    * check ports, ``netstat -an | grep -w LISTEN``
    * check which services use the port, ``cat /etc/services | grep -w PORT``
    * [Required Ports](https://kubernetes.io/docs/reference/ports-and-protocols/)
    * use third-party tools to add extra layer of security such as Cisco ASA, Juniper NGFW,
    Barracuda NGFW, Fortinet
    * **UFW (Uncomplicated Firewall)**
        - interface for ``iptables``
        - ``ufw status``, ``ufw default allow outgoing``, ``ufw default deny incoming``
        - ``ufw allow from 172.16.238.5 to any port 22 proto tcp``
        - ``ufw allow from 172.16.100.0/28 to any port 80 proto tcp``
        - ``ufw deny 8080``, ``ufw enable``
        - ``ufw delete deny 8080`` or ``ufw delete 5`` based on ``ufw status numbered`` line number
* use IAM roles to restrict users in the cloud environment

Seccomp in K8s
--------------
    * to check seccomp configuration
        - ``kubectl run amicontained --image=r.j3ss.co/amicontained amicontained -- amicontained``
    * when container is run as a pod, only 21 syscalls are blocked and mode 0
    * ``allowPrivilegeEscalation``, restrict process inside container from having more privilege
    * custom profiles must be in ``/var/lib/kubelet/seccomp/profiles``
    * to audit the syscalls made by application
        - use ``"defaultAction": "SCMP_ACT_LOG``, logs will be under ``/var/log/syslog``
        - can also use tracee
    * the key is to find all the syscalls made by the application and block all others
    * can help isolate the pod and secure the cluster

    .. code-block:: yaml

       # pod-definition.yml
       spec:
         securityContext:
           seccompProfile:
             type: RuntimeDefault # default is Unconfined
             # for custom profile
             type: Localhost
             localhostProfile: profiles/custom.json
         containers:
         - image: r.j3ss.co/amicontained
           args:
           - amicontained
           securityContext:
             allowPrivilegeEscalation: false



AppArmor in K8s
---------------
    - AppArmor kernel module must be enabled on the node that the pods will run
    - profiles must be loaded, CRI should support AppArmor
    - still in beta state

    .. code-block:: yaml

       # pod-definition.yml
       metadata:
         annotations:
           container.apparmor.security.beta.kubernetes.io/container1: localhost/myprofile



Capabilities
------------
    * super user privileges divided into distinct units
    * processes are assigned a subset of capabilities
    * check needed capabilities of a command, ``getcap /usr/bin/ping``
    * check needed capabilities of a process, ``getpcaps 779``
    * containers using docker runtime has only 14 capabilites by default

    .. code-block:: yaml

       # pod-definition.yml
       spec:
         containers:
         - securityContext:
             capabilities:
               add: ["SYS_TIME"]
               drop: ["CHOWN"]



Minimize base image footprint
-----------------------------
    * base image: an image that is built from scratch, can also call a parent image as base
    * do not build images that combine multiple applications
    * each image must solve only one problem
    * the images can then be combined to form a single service, and each application can be
    scaled without worrying about the others
    * do not persist state inside the container
    * use official, frequently updated and minimal images as base
    * only install necessary packages, rmeove shells/package managers/tools
    * maintain different images for different environment, development with debug tools and
    lean images for production
    * use multi-stage builds
    * distroless docker images
        - contain only application and runtime dependencies
        - ``gcr.io/distroless``
    * minimal image is less vulnerable

Image Policy Webhook
--------------------
    * to ensure images are only pulled from approved registries
    * deploy Admission Webhook server, enable and configure ``ImagePolicyWebhook`` to communicate
    it using `AdmissionConfiguration` file
    * ``defaultAllow``, allow by default if Admission Webhook server is down
    * need to pass ``--admission-control-config-file`` in kube-apiserver

    .. code-block:: yaml

       # admission-config.yml
       apiVersion: apiserver.config.k8s.io/v1
       kind: AdmissionConfiguration
       plugins:
       - name: ImagePolicyWebhook
         configuration:
           imagePolicy:
             kubeConfigFile: my-kubeconfig-file
             allowTTL: 50
             denyTTL: 50
             retryBackoff: 500
             defaultAllow: true
   
       # my-kubeconfig-file
       clusters:
       - name: name-of-remote-imagepolicy-service
         cluster:
           certificate-authority: /path/to/ca.pem
           server: https://images.example.com/policy
       users:
       - name: name-of-api-server
         user:
           client-certificate: /path/to/cert/pem
           client-key: /path/to/key.pem


`back to top <#kubernetes>`_

Tools
=====

* `kubectx`_, `kubesec`_, `Trivy`_, `Weaveworks`_, `Helm`_, `CIS Benchmark`_, `kube-bench`_
* `OPA`_, `gVisor`_, `Kata Containers`_, `Falco`_

kubectx
-------
    * easier way to switch contexts, useful in multi-cluster environment
    * [kubectx](https://github.com/ahmetb/kubectx)

    .. code-block:: sh

       # switch to another cluster that's in kubeconfig
       kubectx minikube
   
       # switch back to previous cluster
       kubectx -
   
       # create an alias for the context
       kubectx dublin=gke_ahmetb_europe-west1-b_dublin
   
       # change the active namespace on kubectl
       kubens kube-system
   
       # go back to the previous namespace
       kubens -



kubesec
-------
    * static analysis: review resource files and enforce policies in development
    * helps analyze the given resource file and returns a score with critical issues found
    * ``kubesec scan pod.yml``, using binaries
    * ``curl -sSX POST --data-binary @"pod.yml" https://v2.kubesec.io/scan``, on hosted server
    * ``kubesec http 8080``, run locally
    * [kubesec](https://github.com/controlplaneio/kubesec)

Trivy
-----
    * criteria to be in CVE (Common Vulnerabilities an Exposures)
        - allowing attacker to bypass security checks
        - severity score from 0-10 (none, low, medium, high, critical)
    * scan container images for CVE by Aqua Security
    * ``triby image nginx:1.12``
    * ``trivy image --severity CRITICAL,HIGH nginx:1.12``, only show critical and high
    * ``trivy image --ignore-unfixed nginx:1.12``, only show that can be immediately fixed
    * can scan tar, ``docker save nginx:1.12 > nginx.tar``, ``trivy image --input nginx.tar``
    * configure Admission Controllers to scan images, have own repository with pre-scanned
    images, integrate scanning into CI/CD pipeline
    * [Trivy](https://github.com/aquasecurity/trivy)

Weaveworks
----------
    * solution that makes creating network models easy
    * deploy agent or service on each node
    * each agent store the topology of entire network setup
    * Weave creates its own bridge named "WEAVE" on each node and assign IP addresses
    * makes sure that pods get correct routes configured to reach the agents
    * agents encapsulate and decapsulate the packets between each other
    * Weave and its peers can be deployed as services/daemons on each node manually
    * deploy as pods if the cluster is already setup with right configurations with
        - peers are deployed as daemonsets and can view logs using ``kubectl``

        .. code-block:: sh

           kubectl apply -f \
           "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"


    * by default, uses IP range 10.32.0.0/12
    * the peers decide to split IP addresses equally between them and assign one portion to
    each node

Helm
----
    * package manager for K8s
    * looks multiple objects of the application as a group
    * keeps track of all objects used by the application
    * do not need to micromanage each object
    * only need to tell what package to act on
    * automatically knows what objects to change, based on package name, even if there are
    hundreds of objects in the package
    * only use single command to install the application with multiple objects
    * helm proceeds to add every necessary objects to K8s
    * can specify desired values at install time
    * ``values.yml``: single file to declare every custom setting instead of multiple YAML files

    .. code-block:: yaml

       # first convert definition files to templates
   
       # templates/deployment.yml
       spec:
         template:
           spec:
             containers:
             - image: {{ .Values.image }}
               name: wordpress
   
       # templates/secret.yml
       data:
         key: {{ .Values.passwordEncoded }}
   
       # templates/pv.yml
       spec:
         capacity:
           storage: {{ .Values.storage }}
   
       # templatespvc.yml
       spec:
         resources:
           requests:
             storage: {{ .Values.storage }}
   
       # {{ .Values.* }} will be fetched from values.yml
   
   
       # values.yml
       image: wordpress
       storage: 20Gi
       passwordEncoded: encoDedPasSwoRd


    * Helm Chart
        - combination of templates, ``values.yml`` and Chart.yml, which has info about the chart
          itself
        - single chart may be used to deploy simple application
        - can create own chart or search from [ArtifactHUB](https://artifacthub.io/), which
          is a repository for Helm Charts
        - ``helm search hub wordpress`` searches from ArtifactHUB
        - other repositories such as [Bitnami]()
        - ``helm repo add bitnami https://charts.bitnami.com/bitnami``: must add repository to
          search from other than ArtifactHUB
        - ``helm search repo wordpress``: can now search
        - ``helm repo list`` to list repos
        - release: each installation of a chart
        - each release has a release name
        - can install multiple releases and each release is independent of each other

    .. code-block:: sh

       helm install release-1 bitnami/wordpress
       helm install release-2 bitnami/wordpress
   
       # helm knows which objects need to be upgraded
       helm upgrade release-2
   
       helm rollback release-1
   
       # no need to delete each object
       helm uninstall release-2
   
       # only download and extract
       helm pull --untar bitnami/wordpress
   
       # install local chart
       helm install release-3 ./wordpress


    * installation prerequisites: kubectl and K8s cluster with right credentials
    * [Helm](https://helm.sh/docs/intro/install/)

CIS Benchmark (Center for Internet Security)
--------------------------------------------
    * some necessary security benchmark
        - physical access (e.g usb ports)
        - user access: what users, which users, disable root user, use sudo if required, but
          only those who are configured to
        - network: configure firewall, iptables, open only required ports
        - services: enable only necessary services
        - filesystems: right permission of files, disable unused file systems
        - auditing, logging
    * CIS provides cyber security benchmarks for over 25 different vendors
    * OS, cloud, mobile, network, desktop and server softwares, databases
    * benchmarks have instructions for various security recommendations
    * each recommendation explains why the threat exists and how to prevent it
    * also provides tools to run assessments and remediate
    * e.g CIS-CAT tool helps in automated assessment of the server and output result in html
    * CIS-CAT Lite tool only support for Windows 10, Ubuntu, Google Chrome, macOS
    * **CIS benchmark for K8s**
        - document contains recommendations for components, details to check current status
          and necessary commands to remediate
        - CIS-CAT Pro tool is required
    * [CIS](https://www.cisecurity.org/)
on the master node and set the --terminated-pod-gc-threshold to an appropriate threshold,
for example:
* -terminated-pod-gc-threshold=10

kube-bench
----------
    * open source tool from Aqua Security to perform automated assessments
    * test according to recommendations from CIS
    * can deploy as container, as a pod in K8s, install binaries or compile from source
    * [kube-bench](https://github.com/aquasecurity/kube-bench)

OPA (Open Policy Agent)
-----------------------
    * takes care of authorization
    * applications don't need to implement authorization in the code
    * deploy in the environment with a set of policies
    * applications first reach OPA and it verifies the request
    * it returns allowed or denied error message and services process the request
    * runs on port 8181 by default
    * policies are defined in ``rego`` language
    * also provides policy testing framework


    # example.rego
    package httpapi.authz
    import input
    default allow = false
    allow {
    # all conditions need to be true
        input.path == "home"
        input.user == "john"
    }


    .. code-block:: sh

       # installation
       curl -L -o opa https://openpolicyagent.org/downloads/v0.44.0/opa_linux_amd64_static
       chmod 755 ./opa
       ./opa run -s
   
       # load policy
       curl -X PUT --data-binary @example.rego http://localhost:8181/v1/policies/example1
   
       # list policies
       curl http://localhost:8181/v1/policies
   
       # test policy
       opa test . -v


    * [OPA at Netflix](https://www.youtube.com/watch?v=R6tUNpRpdnY), [OPA Deep Dive](https://www.youtube.com/watch?v=4mBJSIhs2xQ)

gVisor
------
    * tool made by Google, allows additional layer between containers and the kernel
    * when a process in the container wants to make a syscall to the kernel, it makes to the
    gVisor
    * two components: Sentry and Gofer
    * **Sentry**
        - independent application level kernel
        - intercept syscalls made by container application
        - has limited syscalls
    * **Gofer**
        - file proxy for containerized apps to access the system files
        - Sentry communicates to it if the application needs to access files
    * use its own network stack, so that the container doesn't need to interact directly with
    OS network code
    * not all applications work with gVisor, and more cpu usage as syscalls are processed by
    middleman
    * uses Runsc
    * to be used in K8s cluster, create ``RuntimeClass`` object
    * container cannot be seen as a process from the node

    .. code-block:: yaml

       # gvisor.yml
       apiVersion: node.k8s.io/v1
       kind: RuntimeClass
       metadata:
         name: gvisor
       handler: runsc
   
       # gvisor-nginx.yml
       spec:
         runtimeClassName: gvisor



Kata Containers
---------------
    * process the syscalls into its own VM kernel
    * each container has dedicated VM kernel
    * VM kernels are optimized, but still has slight performance impact
    * won't be able to run in cloud environments, as many CSPs do not provide nested
    virtualization, but can be manually enable in GCP
    * useful in on-premises environment
    * uses kata-runtime
* since kata-runtime and Runsc are OCI compliant, can use with docker
    * ``docker run --runtime kata -d nginx``
    * ``docker run --runtime runsc -d nginx``

Falco
-----
    * if there is a breach, identify as soon as possible
    * analyse the syscalls and filter suspicious ones, and can even notify
    * needs to see what syscalls are made by application from user space to kernel
    * can deploy as kernel module but some K8s providers do not allow as it is intrusive, so
    can make use of eBPF
    * syscalls are analyzed by sysdig libraries in user space
    * events are filterd by policy engine by using pre defined rules and output and alert
    * using Falco as a service on the node directly isolates it from K8s and still continue
    to detect in case of compromise
    * can also deploy as daemonset on the nodes using helm charts
    * installed directly on host
        - ``systemctl status falco``, on host
        - ``journalctl -fu falco``, on nodes, inspect events
    * sysdig filters
        - ``container.id``, ``proc.name``, ``fd.name``
        - ``evt.type``, ``user.name``, ``container.image.repository``
    * ``priority`` values
        - ``DEBUG``, ``INFORMATIONAL``, ``NOTICE``, ``WARNING``
        - ``ERROR``, ``CRITICAL``, ``ALERT``, ``EMERGENCY``
    * use ``list`` instead of making rules for each process
    * use ``macro`` to write simpler conditions
    * main falco configuration file, ``/etc/falco/falco.yml``
        - ``rules_file``, define rules files to be used, order matters, the last file is used
        - ``json_out``, logs events in json, false by default, so logs as text
        - logging options, ``log_stderr``, ``log_syslog``, ``log_level``
        - ``priority``, related to falco rules, minimum to use all events, everything higher
          will be logged
        - ``stdout_output``, logs to stdout by default, can have multiple output channels
        - ``file_output``, logs to a file
        - ``program_output``, send output to programs
        - ``http_output``, send output to http endpoints
    * main rules file, ``/etc/falco/falco_rules.yml``
        - contain all built-in rules, macros and lists
        - check which file is used, ``journalctl -fu falco``
        - changes to the file will be overwritten when falco updates
    * put change or custom rules in ``/etc/falco/falco_rules.local.yml``
    * hot reload, ``kill -1 $(cat /var/run/falco.pid)``
    * [Falco](https://falco.org/docs/), [Falco charts](https://github.com/falcosecurity/charts)

    .. code-block:: yaml

       # falco-rules.yml
       - rule: Detect shell inside container # name of rule
         desc: Alert if shell is opened inside container # detailed description of rule
         # when to filter events matching
         condition: container and proc.name in (linux_shells)
         output: Bash Shell Opened (user=%user.name %container.id) # output to ge generated
         priority: WARNING # severity of event
       - list: linux_shells
         items: [bash, zsh, ksh, sh, csh]
       - macro: container
         condition: container.id != host
   
       # custom-falco-rules.yml


`back to top <#kubernetes>`_

Exam Guide
==========

* certs are made by CNCF in collaboration with The Linux Foundation
* `exam instructions`_, `curriculum`_, `handbook`_
* discount code: DEVOPS15

CKAD
----
    * to be able to design and build cloud-native applications for K8s
    * 2-hours online performance based exam with hands-on labs, 19 questions
    * allow to access [K8s Documentation](https://kubernetes.io/docs/home/) page during exam
    * can answer questions in any order
    * finish as many questions as possible, max attempt 3 times per question and skip if cannot
    solve, then comeback later
    * get good with YAML, indentation doesn't need to look pretty
    * remember to use aliases for commands (po, rs, deploy, svc, ns, netpol, pv, pvc, sa)
    * require only minimal knowledge of logging and monitoring
    * [CKAD](https://www.cncf.io/certification/ckad/), [tips](https://medium.com/@harioverhere/ckad-certified-kubernetes-application-developer-my-journey-3afb0901014)

CKA
---
    * to be able to design, build and administer K8s cluster
    * allow to access [K8s Documentation](https://kubernetes.io/docs/home/) page during exam
    * can use any CNI plugin if not specified, but cannot access the plugin documents
    * [CKA](https://www.cncf.io/certification/cka/)

CKS
---
    * must have CKA certificate and certificate must be active to take the exam
    * allow to access [K8s Documentation](https://kubernetes.io/docs/home/) page during exam
    * may ask to copy seccomp profiles
    * [CKS](https://www.cncf.io/certification/cks/)

`back to top <#kubernetes>`_

Additional Resources
====================

* `Kubernetes the hard way`_
* `K8s Documentation`_, `Glossary`_
* `kubectl conventions`_, `kubectl overview`_, `kubectl cheat sheet`_, `kubectl commands`_
* `Secrets`_
* `Init Containers`_
* `Ingress`_, `K8s DNS`_
* `Storage`_, `Object Management`_
* `API Conventions`_, `Changing the API`_
* `Addons`_
* `KodeKloud CKA notes`_, `KodeKloud CKS notes`_

`back to top <#kubernetes>`_
