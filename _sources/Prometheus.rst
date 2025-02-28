==========
Prometheus
==========

1. `What is Prometheus?`_
2. `Architecture`_
3. `Installation`_
4. `Configuration`_
5. `Security`_
6. `Metrics`_
7. `Promtools`_
8. `Alertmanager`_
9. `Data Storage`_
10. `PromQL`_
11. `Visualization`_
12. `Monitoring`_
13. `Service Discovery`_
14. `Pushgateway`_

What is Prometheus?
===================


Obervability
------------
    * to understand and measure the state of a system based on data generated
    * perform actions depending on the data
    * give better insights of the system to troubleshoot and monitor performance
    * logs
        - records of event with timestamp and information
        - output can be difficult to understand
    * traces
        - follow operations flow through systems
        - help understand how systems work together
        - each trace has a trace ID
        - span: individual events forming a trace, track start time, duration and parent ID
    * metrics
        - information about system's state using numerical values
        - can visualize data collected
        - contain metric name, dimensions, value and timestamp
* open source monitoring tool for highly dynamic container environments, only handles **metrics**
* can also be used in non-container infrastructure
* constantly monitor all the services and alert when crash or identify problems ahead
* e.g., check memory usage and notify admin when near limit

Advantages
----------
    * reliable, stand-alone and self-containing
    * doesn't depend on network storage or other remote services
    * works even if other parts of infrastructure are broken
    * don't need to setup extensive infrastructure to use

Disadvantages
-------------
    * deep learning curve to correctly configure and query metrics as it's not well-documented
    * difficult to scale
    * single node is less complex but limits on number of metrics that can be monitored
    * need to increase Prometheus server capacity or limit number of metrics
* compatible with both docker and K8s
* can be easily deployed in K8s or other container environments
* provide **cluster node resource monitoring** out of the box
    * once deployed on K8s, starts gathering metrics data on each K8s node server without extra
    configuration

SLI
---
    * Service Level Indicator
    * metrics to measure quality of the service provided
    * e.g latency, error rate, throughput, availability
    * not accurate enough to measure user's experience

SLO
---
    * Service Level Object
    * target value or range for SLI
    * e.g latency < 100ms, 99.9% availability
    * directly related to user experience
    * cannot achieve perfection but be reliable

SLA
---
    * Service Level Agreement
    * contract between vendor and user
    * guarantee certain SLO

`back to top <#prometheus>`_

Architecture
============


Prometheus server
-----------------
    * main component that does the actual monitoring
    * exposed at default port 9090
    * *storage*: time series database that stores metrics data
    * *retrieval*: data retrieval worker responsible for getting metrics from resources
    * *http server*: api that accepts queries for the stored data and used to visualize data

targets
-------
    * resources that Prometheus monitors
    * e.g., Linux/Windows server, Apache server, single app, services like databases
    * each target has units of monitoring
    * Linux server: CPU status, memory/disk usage
    * application: exceptions count, requests count, request duration

metrics
-------
    * unit to monitor for a specific target
    * saved into Prometheus database component
    * defined in human readable text-based format
    * metrics entries has *TYPE* and *HELP* attributes
    * Counter TYPE: how many times x happened?
    * Gauche TYPE: metrics that can go up and down, what is the current value of x now?
    * Histogram TYPE: how long or how big for size of request?

collecting metrics data from targets
------------------------------------
    * pull metrics data from targets from HTTP endpoint (e.g., hostaddress/metrics)
    * targets must expose '/metrics' endpoint
    * data available at endpoint must be in format

exporter
--------
    * extra component for services that don't have native Prometheus endpoint
    * exposed at default port 9100
    * script or service that fetches metrics from target
    * converts the data to correct Prometheus format
    * exposes the converted data at its own '/metrics' endpoint
    * e.g. MySQL elasticsearch, Linux server build tools/ node exporters, Apache, HAProxy
    * also available as docker images and as Prometheus client libraries for monitoring
    applications

monitoring systems
------------------
    * **pull system**
        - Prometheus default model
        - multiple Prometheus instances can pull metrics data
        - can detect if service is up and running
    * **push system**
        - apps and servers are responsible for pushing metrics data to centralized collection
         platform (e.g., Amazon Cloud Watch, Relic)
        - can cause high load of network traffic as services constantly push data
        - service not pushing metrics can be caused by many reasons other than service not
        running
        - can't have insight what is happening to the service
        - usually used in event-based systems
    * **pushgateway**
        - for a target that runs only for a short time
        - e.g., batch job or scheduled job that cleans up data
        - using pushgateway in Prometheus should be an exception

`back to top <#prometheus>`_

Installation
============


As systemd service
------------------
    * **Prometheus**

        .. code-block:: sh

           # create user to only start prometheus, user cannot log in
           sudo useradd --no-create-home --shell /bin/false prometheus
   
           # to store prometheus.yml
           sudo mkdir /etc/prometheus
   
           # to store data
           sudo mkdir /var/lib/prometheus
   
           # update owner
           sudo chown prometheus:prometheus /etc/prometheus
           sudo chown prometheus:prometheus /var/lib/prometheus
   
           # download and move binaries to PATH
           # move consoles and console_libraries to /etc/prometheus
           # update owner to all folders and binaries
           # also update owner for prometheus.yml file
   
           # start as prometheus user
           sudo -u prometheus /usr/local/bin/prometheus \
           --config.file /etc/prometheus/prometheus.yml \
           --storage.tsdb.path /var/lib/prometheus \
           --web.console.templates=/etc/prometheus/consoles \
           --web.console.libraries=/etc/prometheus/console_libraries
   
           # create service file
           sudo vi /etc/systemd/system/prometheus.service
   
           # start and enable as service
           sudo systemctl daemon-reload
           sudo systemctl start prometheus
           sudo systemctl enable prometheus


        .. code-block:: sh

           # example prometheus.service file
   
           [Unit]
           Description=Prometheus
           Wants=network-online.target
           After=network-online.target
   
           [Service]
           User=prometheus
           Group=prometheus
           Type=simple
           ExecStart=/usr/local/bin/prometheus \
               --config.file /etc/prometheus/prometheus.yml \
               --storage.tsdb.path /var/lib/prometheus \
               --web.console.templates=/etc/prometheus/consoles \
               --web.console.libraries=/etc/prometheus/console_libraries
   
           [Install]
           WantedBy=multi-user.target


    * **Node Exporter**

        .. code-block:: sh

           # move binary to PATH
   
           # create user
           sudo useradd --no-create-home --shell /bin/false node_exporter
   
           # update owner to binary
   
           # create service file
           sudo vi /etc/systemd/system/node_exporter.service
   
           # start and enable as service
           sudo systemctl daemon-reload
           sudo systemctl start node_exporter
           sudo systemctl enable node_exporter


        .. code-block:: sh

           # example node_exporter.service file
           [Unit]
           Description=Node Exporter
           Wants=network-online.target
           After=network-online.target
   
           [Service]
           User=node_exporter
           Group=node_exporter
           Type=simple
           ExecStart=/usr/local/bin/node_exporter
   
           [Install]
           WantedBy=multi-user.target



As Docker container
-------------------
    * **Prometheus**

        .. code-block:: yaml

           version: "3.9"
   
           services:
             prometheus:
               image: prom/prometheus:latest
               container_name: prometheus
               ports:
                 - "9090:9090"
               volumes:
                 - /config/path/:/etc/prometheus/


    * **Node Exporter**

        .. code-block:: yaml

           version: "3.9"
   
           services:
             node_exporter:
               image: prom/node-exporter:latest
               container_name: node_exporter
               command:
                 - "--path.rootfs=/host"
               network_mode: host
               pid: host
               restart: unless-stopped
               volumes:
                 - "/:/host:ro,rslave"


`back to top <#prometheus>`_

Configuration
=============

* configured in **promethus.yml** file
    * define which target at what interval
* Prometheus uses service discovery mechanism to find the target endpoints
* configured to monitor itself by default

.. code-block:: yaml

   # default config file
   
   # how often, default parameters
   global:
     scrape_interval: 15s
     evaluation_interval: 15s  # how often to evaluate rules
   
   # what resources to monitor
   scrape_configs:
     - job_name: prometheus  # monitor itself as Prometheus has /metrics exposed
       scrape_interval: 10s  # override default value
       scheme: https # default is http
       metrics_path: /custom/path
       static_configs:
         - targets: ['localhost:9090']
   
   # AlertManager configuration
   alerting:
   
   # aggregate metric values or create alerts
   rule_files:
     # - "first.rules"
     # - "second.rules"
   
   # remote read/write feature
   remote_read:
   remote_write:
   
   # storage
   storage:



Docker metrics
--------------
    * only docker engine metrics, no container metrics
    * create /etc/docker/daemon.json and reload service

    .. code-block:: json

       {
         "metrics-addr" : "127.0.0.1:9323",
         "experimental" : true
       }



cAdvisor metrics
----------------
    * give container metrics

    .. code-block:: yaml

       services:
         cadvisor:
           image: gcr.io/cadvisor/cadvisor:latest
           container_name: cadvisor
           ports:
           - 8080:8080
           volumes:
           - /:/rootfs:ro
           - /var/run:/var/run:rw
           - /sys:/sys:ro
           - /var/lib/docker/:/var/lib/docker:ro


`back to top <#prometheus>`_

Security
========


Encryption
----------
    * data is not encrypted by default
    * exporters should use TLS

        .. code-block:: sh

           sudo openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 \
           -keyout node_exporter.key -out node_exporter.crt \
           -subj "/C=US/ST=California/L=Oakland/O=MyOrg/CN=localhost" \
           -addext "subjectAltName = DNS:localhost"


    * exporter config.yml

        .. code-block:: yaml

           tls_server_config:
               cert_file: node_exporter.crt
               key_file: node_exporter.key


    * start exporter with TLS config: ``./node_exporter --web.config=config.yml``
    * copy ``node_exporter.crt`` file to ``/etc/prometheus`` and update config

        .. code-block:: yaml

           scrape_configs:
               - job_name: "node"
                 scheme: https
                 tls_config:
                   ca_file: /etc/prometheus/node_exporter.crt
                   insecure_skip_verify: true # for self-signed certs



Authentication
--------------
    * anyone can scrape metrics without authentication
    * need to create hash of the password, can use apache2-utils, httpd-tools or any language

        .. code-block:: sh

           # using apache2-utils
           htpasswd -nBC 12 "" | tr -d ':\n'


    * update node_exporter ``config.yml``

        .. code-block:: yaml

           basic_auth_users:
               USER_NAME: HASHED_PASSWORD


    * update ``prometheus.yml``

        .. code-block:: yaml

           scrape_configs:
               - job_name: "node"
                 basic_auth:
                   username: USER_NAME
                   password: PLAIN_TEXT_PASSWORD


`back to top <#prometheus>`_

Metrics
=======


Properties
----------
    * Name
        - can contain ASCII letters, numbers, underscores and colons
        - colons are reserved for recording rules
    * Labels
        - key-value pairs, can have more than one label
        - provide information and allow to split metric by criteria
        - can contain ASCII letters, numbers and underscores
        - metric name is also a label, ``__name__=metric_name``
        - labels with two underscores are internal labels
        - every metric is assigned 2 labels by default, instance and job
    * Value
* also store timestamp, in unix timestamp

Time series
-----------
    * stream of timestamped values sharing same metric and labels

Attributes
----------
    * Type
        - type of prometheus metrics
        - counter: how many times did X happen, only increase
        - gauge: current value of X, can go up or down
        - histogram: total of metrics, allow to group observations based on ranges
        - summary: similar to histogram, do not need to define quantiles ahead of time
    * Help
        - description of metrics

ReLabeling
----------
    * to classify/filter targets and metrics by rewriting their label set
    * relabel_configs: occurs before scrape and only has access to labels added by service
    discovery
    * metric_relabel_configs: occurs after scrape and has access to all metrics and labels
    scraped, configuration is same as relabel_configs
    * keep: scrape target if it has source_labels, targets that do not match will be dropped,
    use to keep one or two labels and drop everything else
    * drop: not scrape target, use to drop one or two labels and keep everything else
    * all labels beginning with __ will not be shown as target labels and discarded at the end
    of relabeling
    * labelmap: to keep one or more of labels beginning with __ and only modifies the label
    name, not the value

    .. code-block:: yaml

       scrape_configs:
           - job_name: EC2
             relabel_configs:
               - source_labels: [l1, l2] # array of labels to match on
                 regex: v1;v2 # to match specific value, will match l1=v1 and l2=v2
                 action: keep|drop|replace
                 separator: "-" # can use regex of "v1-v2" instead of semicolon
   
       # convert {__address__=1.1.1.1:80} to {ip=1.1.1.1}
       scrape_configs:
           - job_name: EC2
             relabel_configs:
               - source_labels: [__address__]
                 regex: (.*):.* # assign everything before : into group
                 target_label: ip # name of new label
                 action: replace
                 replacement: $1 # referenced the first group from regex
   
       # drop a label
       scrape_configs:
           - job_name: EC2
             relabel_configs:
               - regex: __meta_ec2_owner_id
                 action: labeldrop
   
       # keep a label, opposite of drop, labels that don't match will be dropped
       scrape_configs:
           - job_name: EC2
             relabel_configs:
               - regex: instance|job # only instance and job labels will be kept
                 action: labelkeep
   
       # convert {__meta_ec2_owner_id="123"} to {ec2_owner_id="123"} and others as well
       scrape_configs:
           - job_name: EC2
             relabel_configs:
               - regex: __meta_ec2_(.*)
                 action: labelmap
                 replacement: ec2_$1


`back to top <#prometheus>`_

Promtools
=========

* utility tool to validate configuration, rules, metrics format
* perform queries and debug Prometheus server, and perform uni tests on rules

.. code-block:: sh

   # check config
   promtool check config /etc/prometheus/prometheus.yml


`back to top <#prometheus>`_

Alertmanager
============

* responsible for alerting via different channels (e.g., email, slack)
* alerts get fired when the rules are met, which are defined as standard PromQL expressions
* Prometheus servers only trigger alerts, Alertmanager is responsible for sending notifications
* one Alertmanager support multiple Prometheus servers
* alerting rules are similar to recording rules

.. code-block:: yaml

   groups:
       - name: node
         interval: 15s
         rules:
           - record:
           - alert: LowMemory
             expr: node_memory_memFree_percent < 20
           - alert: NodeDown
             expr: up{job="node"} == 0 # only works for targets with job=node lables
             for: 5m # expression must be true for given period before firing alert


* Inactive: expression has not returned any results
* Pending: expression returned results, but not long enough
* Firing: active for more than the defined for clauses
* can add labels to classify and match specific alerts

    .. code-block:: yaml

       groups:
           - name: node
             interval: 15s
             rules:
               - alert: LowMemory
                 expr: node_memory_memFree_percent < 20
                 labels:
                   KEY: VALUE


* can have annotations to only provide additional information
    * templated using Go templating language
    * alert labels: ``{{.Labels}}``
    * instance labels: ``{{.Labels.instance}}``
    * firing sample value: ``{{.Value}}``

    .. code-block:: yaml

       groups:
           - name: node
             interval: 15s
             rules:
               - alert: LowMemory
                 expr: 100 * node_filesystem_free_bytes < 70
                 annotations:
                   description: "filesystem {{.Labels.device}} on {{.Labels.instance}} is low,
       current available space is {{.Value}}"



Alertmanager Architecture
-------------------------
    * dispatcher->inhibition->silencing->routing->notifications<->receivers
    * dispatching group first receive the alerts
    * inhibition node allows to suppress certain alerts if others exist
    * silencer allows to mute alerts at specific moments, e.g., when maintaining servers
    * routing decides what alerts get sent to who through what integration
    * notifications is responsible for having third-party integrations and sending to users
* exposed on default port 9093

    .. code-block:: yaml

       alerting:
           alertmanagers:
               - static_configs:
                   - targets:
                       - alertmanager1:9093
                       - alertmanager2:9093



Alertmanager Configuration
--------------------------
    * global: global config for all sections
    * route: set of rules to match alerts and receivers, can have sub-routes, will default to
    parent route if sub-routes do not match
    * receivers: notifiers to forward alerts to users
    * to update config, must restart, send SIGHUP or send HTTP POST to /-/reload endpoint
    * **Notification Templates**
        - messages from notifiers can be configured with Go templating system
        - GroupLabels, CommonLabels, CommonAnnotations, ExternalURL, Status, resolved,
        Receiver, Alerts (Labels, Annotations, status, StartsAt, EndsAt)

    .. code-block:: yaml

       global:
           smtp_smarthost: 'mail.example.com'
           smtp_from: 'test@example.com'
   
       route:
           receiver: staff # default/fallback route
           group_by: ['alertname', 'job']
           routes:
               - match_re:
                   job: (node|windows)
                 receiver: email
                 continue: true # will continue down to see if it matches
               - matchers:
                   job: kubernetes
                 receiver: slack
   
       receivers:
           - name: 'slack'
             slack_configs:
               - api_url: https://hooks.slack.com/test
                 channel: '#alerts'


`back to top <#prometheus>`_

Data Storage
============

* stores data locally or optionally on remote storage system
* data is stored in custom time series format
* Prometheus data cannot be written to a relational database
* data can be queried through server api using PromQL Query Language
* can use dashboard UI to view status of target from Prometheus server via PromQL
* can also use data visualization tools like Grafana that also uses PromQL

`back to top <#prometheus>`_

PromQL
======

* Prometheus Query Language, to query metrics within Prometheus

    .. code-block:: sh

       http_requests_total{status!~"4.."} # query all HTTP status codes execpt 4xx
       rate(http_requests_total[5m])[30m:] # 5min rate of http_requests_total metric for past 30mins



Types
-----
    * string
        - currently unused
    * scalar
        - numeric floating point value
    * instant vector
        - set of time series containing single sample for each series, all share same
        timestamp
        - e.g query of ``METRICS`` returning multiple ``METRICS{LABELS} VALUE`` with same
        ``TIMESTAMP``
    * range vector
        -set of time series containing range of data points over time for each time series
        - e.g query of ``METRICS[time]`` returning multiple ``METRICS{LABELS} VALUE`` with
        different ``TIMESTAMP``

Label Selectors & Matchers
--------------------------
    * **Matchers**
        - to match specific labels
        - ``=``: equal, ``!=``: not equal, ``=~``: regex match, ``!~``: negate regex match
    * **Selectors**
        - can use multiple selectors by separating with comma
        - e.g ``node_filesystem_avail_bytes{instance="node1", device!="tmpfs"}``
        - e.g ``node_filesystem_avail_bytes{instance="node1", device!="tmpfs"}[3m]``, using
        range vectors

Modifiers
---------
    * a query returns most recent value by default
    * can use modifier to get past data
    * e.g offset modifier: ``node_filesystem_avail_bytes{instance="node1"} offset 5m``
    * to specific point in time
        - e.g @ modifier: ``node_filesystem_avail_bytes{instance="node1"} @2132151523``, unix
        timestamp
    * can combine multiple modifiers
        - modifiers order does not matter
        - e.g ``node_filesystem_avail_bytes{instance="node1"} @2132151523 offset 3m``
    * can combine with range vectors
        - e.g ``node_filesystem_avail_bytes{instance="node1"}[2m] @2132151523 offset 3m``

Operators
---------
    * metric name is removed from output when it is modified
        - e.g ``node_filesystem_avail_bytes{instance="node1"} + 10`` will output
        ``{instance="node1"} VALUE``
    * Arithmetic: +, -, \*, /, %, ^
    * Comparison: ==, !==, >, <, >=, <=, bool (return 1 or 0, mostly used for alerts)
    * Logical: or, and, unless
    * Aggregation
        - take an instant vector and aggregate its elements, and result in new instant vector
        - sum, min, max, avg, group, stddev, stdvar, count, count_values, bottomk, topk,
        quantile (determine how many values in distribution are above or below certain limit,
         90% quantile = at what value is 90% of the data less than)
        - can use ``by`` to group labels and aggregate
        - e.g ``sum by(path) (http_requests)``
        - can use ``without`` to ignore labels and aggregate

Vector Matching
---------------
    * vectors with exact same labels get matched and performed operations
        - can use ``ignoring`` to ensure a match between vectors
        - e.g ``http_errors / ignoring(code) http_requests``
        - can use ``on`` to specify exact list of labels to match
        - e.g ``http_errors / on(method) http_requests``
    * one-to-one
        - every element on the left vector try to find single matching element on the right
    * one-to-many
        - elements on the one side can match with multiple elements on the other/many side
        - e.g ``http_errors + on(method) group_left http_requests``, http_errors become many
        side
        - e.g ``http_requests + on(method) group_right http_errors``, http_errors become many
        side

Functions
---------
    * math
        - e.g ceil, floor, abs, round
    * date & time
        - time (returns current time), minute, hour, day_of_week, day_of_month, days_in_month,
         month, year
    * vector
        - take scalar and return instant vector
        - e.g ``vector(4)`` output ``{}4``
    * scalar
        - take vector and return scalar, vector must have exactly one element
        - return ``NaN`` if vector has more than one element
    * sort
        - ascending by default
        - ``sort_desc()`` to sort in descending order
    * rate
        - return per second average rate of change
        - rate: takes last and first value, an average rate over the range, mostly used for
        slow moving counters and alert rules
        - e.g ``rate(http_errors[1m])``, group values in 1m interval, (last - first)/60s and
        return the calculated value
        - irate: takes last and value before last, instant rate, mostly used to graph volatile
        counters
        - e.g ``irate(http_errors[1m])``, group values in 1m interval, (last - before_last)/15s
        and return the calculated value, 15s is the interval between last two values
        - should at least have 4 samples in a range to calculate rate and irate
        - e.g 15s scrape interval 60s window give 4 samples
        - always use rate function first when combining with aggregate

Subquery
--------
    * format: ``<instant query> [<range>:<resolution>] [offset <duration>]``
    * e.g ``max_over_time(rate(http_requests_total[1m]) [5m:30s])``
        - ``rate(http_requests_total[1m]) [5m:30s]``, return range vector with 1m sample range,
        data from last 5m and 30s gap between each query

Histogram
---------
    * have 3 sub metrics: count, sum, bucket
    * histogram_quantile
        - allow to calculate quantiles easily
        - ``histogram_quantile(<percentile>, <histogram metric>)``
        - e.g ``histogram_quantile(0.75, request_latency_seconds_bucket)``, 75% of all requests
        had a latency of VALUE or less
        - can be used to measure SLO, but only approximation as the function use linear
        interpolation
        - for accurate SLO, make sure one bucket has the specific SLO value
    * each histogram bucket is stored as separate time series
    * having too many buckets can result in high cardinality, high memory usage and disk space,
    and slower database insertion
    * can pick bucket sizes, less taxing on client libraries, can select any quantile,
    Prometheus server must calculate quantiles

Summary
-------
    * similar to histogram, work with percentages by default
    * have 3 sub metrics: count, sum, quantile
    * must define quantiles, more taxing on client libraries, only predefined quantiles in
    client can be used, less server-side cost

Recording Rules
---------------
    * allow Prometheus to periodically evaluate PromQL expression and store the result
    * provide aggregate results to be used by others, such as Grafana
    * define in separate file, can use globs to match files, ``/etc/Prometheus/rules/*.yml``
    * changes in rule file require Prometheus server restart
    * rules in a group are evaluated sequentially, one rule can reference previous rules
    * groups run in parallel
    * naming practices
        - ``level:metric_name:operations``
        - level: aggregation level of the metric by the labels
        - operations: list of functions applied to the metric
        - e.g for http_errors with labels of "method" and "path",
        expr: ``sum without(instance) (rate(http_errors{job="api"}[5m]))``,
        record: ``job_method_path:http_errors:rate5m``
    * all rules for a specific job should be in a single group

    .. code-block:: yaml

       # prometheus.yml
       global:
           scrape_interval: 10s
           evaluation_interval: 10s
       rule_files:
           - rules.yml
   
       # rules.yml
       groups:
           - name: GROUP_NAME
             interval: INTERVAL # inherit global by default
             rules:
               - record: RULE_NAME # will be used to perform query
                 expr: PROMQL_EXPRESSION
                 labels:
                   LABEL_NAME: LABEL_VALUE



HTTP API
--------
    * used to integrate with 3rd party tools
    * send POST request to ``http://PROMETHEUS_IP/api/v1/query``
        - e.g ``curl http://PROMETHEUS_IP/api/v1/query --data 'query=PROMQL_EXPRESSION'``
        - e.g ``curl http://PROMETHEUS_IP/api/v1/query --data 'time=TIME_STAMP'``

`back to top <#prometheus>`_

Visualization
=============


Expression Browser
------------------
    * built-in web UI to execute queries and graphs
    * limited functions and should only be used when necessary
    * cannot build custom dashboards

Console Templates
-----------------
    * allow to create custom html pages using Go templating language
    * can embed Prometheus metrics, queries and charts in the templates
    * prebuilt templates in ``/etc/prometheus/consoles``
    * can view at ``http://PROMETHEUS_IP/consoles/TEMPLATE_NAME``

`back to top <#prometheus>`_

Monitoring
==========


Client Libraries
----------------
    * to add instrumentation to application to track and expose metrics for Prometheus
    * expose metrics via "/metrics" path
    * official: Go, Java/Scala, Python, Ruby, Rust
    * other languages are unofficial third-party client libraries
    * can write own client library
    * use labels when defining metrics for application
    * naming convention
        - use snake case, ``library_name_unit_suffix``
        - library: app/library the metric is used for
        - name: description of the metric, can use more than one word
        - unit: use unprefixed base units
        - suffix: avoid count, sum and bucket, total can be used for counter metrics
    * what to instrument
        - online app: number of requests, errors, latency, in-progress requests
        - offline app: amount of queued work, in-progress work, rate of processing, errors
        - batch job (require pushgateway): process time for each stage, overall runtime, last
        completed job time

Monitoring Kubernetes
---------------------
    * it is better to install Prometheus instance on the cluster itself, instead of a separate
    server
    * deploying Prometheus as close to target as possible and can use pre-existng kubernetes
    architecture
    * can monitor applications on the cluster and the cluster itself, such as control-plane
    components, kubelet, kube-state-metrics and node-exporter
    * cluster-level metrics cannot be collected by default
    * must deploy kube-state-metrics container in the cluster for Prometheus to access metrics
    * use daemonSet with node-exporter process for nodes/hosts to expose metrics
    * Prometheus can use Kubernetes API for service discovery
    * manual deployment of Prometheus on Kubernetes can be complex
    * using [Helm chart](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) can be easier, which makes use of [Prometheus Operator](https://github.com/prometheus-operator/prometheus-operator)
        - helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
        - helm repo update
        - helm install prometheus prometheus-community/kube-prometheus-stack
    * **Components installed from Helm chart**
        - stateful sets: Prometheus server, Alertmanager
        - deployments and replica sets: Grafana, Prometheus Operator, kube-state-metrics
        - daemon set: node-exporter
        - all services are ClusterIP
    * can discover targets such as node, service, pod, endpoints
    * using endpoints for service discovery can get any targets in the cluster
    * **Service monitors**
        - define a set of targets for Prometheus to monitor and scrape
        - allow to avoid modifying Prometheus configs directly
        - give declarative Kubernetes syntax to define targets
    * **Applying scrape configs**
        #. modifying Helm chart values of additionalScrapeConfigs
        #. more optimal way of using ServiceMonitor by providing endpoints to scrape, need to
        have same serviceMonitorSelector.matchLabels from Prometheus CRD for Prometheus to
        dynamically discover the ServiceMonitor
    * use PrometheusRule CRD to add rules, need to have same ruleSelector.matchLabels
    * use AlertmanagerConfig CRD to add Alertmanager rules
        - rule files for Kubernetes use camelCase
        - need to add custom Helm value for alertmanagerConfigSelector.matchLabels, and have
        same label in AlertManager rule file

`back to top <#prometheus>`_

Service Discovery
=================

* populate a list of dynamically updated endpoints to scrape
* Prometheus has built-in support for several service discovery mechanisms

File Service Discovery
----------------------
    * can import a list of jobs/targets from a file
    * can integrate with service discovery systems Prometheus doesn't support out of the box
    * support json and yaml files

    .. code-block:: yaml

       scrape_configs:
           - job_name: file-example
             file_sd_configs:
             - files:
                 - 'file-sd.json'
                 - '*.json'


    .. code-block:: json

       [
           {
               "targets": ["node1:2000", "node2:2100"],
               "labels": {
                   "team": "dev",
                   "job": "node"
               }
           },
           {
               "targets": ["localhost:5000"],
               "labels": {
                   "team": "monitoring",
                   "job": "prometheus"
               }
           }
       ]



AWS
---
    * need to provide region, access key and secret key for EC2
    * credentials should be setup with IAM user with ``AmazonEC2ReadOnlyAccess`` policy
    * can extract metadata such as tag name, instance type, vpc id, private ip, etc.
    * use private ip as default instance label

    .. code-block:: yaml

       scrape_configs:
           - job_name: EC2
             ec2_sd_configs:
             - region: r1
               access_key: key1
               secret_key: key2


`back to top <#prometheus>`_

Pushgateway
===========

* act as middleman between batch job and Prometheus server
* batch job push metrics to the push gateway before exiting and Prometheus scrape from the push
gateway like other instances
* can install on any server or same server as Prometheus
* exposed at default port 9091
* configuration is same as others, with extra ``honor_labels``, which allows the metrics to
specify custom labels for the jobs and instance labels, instead of having all labels as
pushgateway

.. code-block:: yaml

   scrape_configs:
       - job_name: pushgateway
         honor_labels: true
         static_configs:
           - targets: ["IP:9091"]


* metrics can be pushed to it using
    * HTTP requests
        - ``http://IP:PORT/metrics/job/JOB_NAME/LABEL1/VALUE1/LBAEL2/VALUE2``
        - ``LABEL/VALUE`` is used as a grouping key, to group metrics together and update
        multiple metrics at once
        - POST: only metrics with same name as newly pushed metrics are replaced
        - PUT: all metrics within specific group are replaced by new ones
        - DELETE: delete all metrics within a group
    * Prometheus Client Libraries
        - push: existing metrics for the job are removed and pushed ones are added, same as
        HTTP PUT
        - pushadd: pushed metrics override existing same ones, others remain unchanged, same
        as HTTP POST
        - delete: delete all metrics for a group

        .. code-block:: python

           from prometheus_client import CollectorRegistry, Gauge, pushadd_to_gateway
   
           registry = CollectorRegistry()
           test_metric = Gauge('test_metric', 'Example metric', registry=registry)
   
           test_metric.set(9)
   
           pushadd_to_gateway('IP:9091', job='batch', registry=registry)


`back to top <#prometheus>`_
