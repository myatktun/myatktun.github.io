===
AWS
===

1. `Job Roles`_
2. `Management with IaC`_
3. `Cloud Models`_
4. `Well-Architected`_
5. `AWS Global Infrastructure`_
6. `Pricing & Support`_
7. `CAF`_
8. `6 Rs`_
9. `Cloud Acquisition`_
10. `Application Integration`_
11. `Compute`_
12. `Containers`_
13. `Cost Management`_
14. `Database`_
15. `Artificial Intelligence`_
16. `Management & Governance`_
17. `Migration & Transfer`_
18. `Networking & Content Delivery`_
19. `Security & Compliance`_
20. `Storage`_
21. `Exam Guides`_
22. `References & External Resources`_

Job Roles
=========


Classic IT Job Roles
--------------------
    * **Architect**
        - design based on business requirements
        - Enterprise or System architect
           + translate business requirements into technical architecture
           + manage teams of designers and engineers
        - Application architect
        - Storage architect
        - Network architect
        - Security architect
    * **System Administrator**
        - install, support and maintain systems and servers
        - monitor performance and network
        - manage users and access control
    * **Application Administrator**
        - install, update and maintain apps
        - works closely with development and technical teams
        - manages documentation of the app
    * **Database Administrator**
        - design, create and maintain databases
        - train employees to use dbs
        - Operations DBA: manages dbs
        - Development DBA: design db architectures
        - Data admin: manage access
    * **Network Administrator**
        - design, install/configure and maintain LAN and WAN
    * **Storage Administrator**
        - install, maintain/test and backup/recover storage systems
        - monitor environment and manage capacity
        - works with other technical teams such as application team
    * **Security Administrator**
        - install and administrate security solutions
        - analyze and implement security policies
        - monitor network traffic and unauthorized access
        - manage tools and perform tests

Cloud Job Roles
---------------
    * **Enterprise Architect**
        - works in business management, infrastructure, security and application infrastructure
        - responsible for delivering cloud services for business
           + obtain business requirements
           + design solution-independent architectures and  present different models
           + validate, refine, expand, manage, monitor and update architectures
    * **Program Manager**
        - ensure that the cloud is managed appropriately
        - manage operational teams
        - monitor cloud metrics, manage service reports
    * **Financial manager**
        - perform cost coding and cost optimization
    * **Infrastructure Architect**
        - design/validate/expand solution-dependent architectures and requirements
        - develop and maintain plans, collaborate with others
    * **Security Architect**
        - specify, design and maintain security requirements
        - design and maintain risk assessment plans and incident response plans
    * **Application Architect**
        - design/validate/expand solution-dependent architectures and requirements
        - perform capacity and scalability requirements
        - provide deep software knowledge
    * **Application Developer**
        - build infrastructure/application, manage app documentation
        - provide training and support to users
    * **DevOps Engineer**
        - build and manage fast and scalable workflows, design automation solutions
        - review and recommend operational improvements
        - develop and maintain change management processes
    * **Operations Engineer**
        - build, monitor and manage cloud infrastructure and shared services
        - collaborate with cloud infrastructure architect
        - ensure that service requirements are met
        - Management: OS management, patch and update, manage templates, document changes, tag
          and review cloud infrastructure, manage capacity, virtual networks, and app
          resiliency
        - Support: provide through monitoring, which also includes compliance programs,
          perform performance tuning and root cause analysis, document reviews
        - Security: manage, monitor and enforce security, implement security policies, perform
          vulnerability testing and risk analysis
        - Infrastructure: build infrastructure/application

`back to top <#aws>`_

Management with IaC
===================

* can also manually manage through AWS management console, AWS cli or AWS apis
* managing with Infrastructure as Code provides reusable, maintainable, extensible and
  testable infrastructure using versioning control and continuous integration and delivery

DevOPs
------
    * first design solution-independent architecture (by enterprise architect)
    * **Dev**
        - design cloud optimized application architecture (by security and application
          architect)
        - write the code (by security operations and app developer)
        - build and compile, test and package application and infrastructure code (by
          devops engineer)
    * **OPs**
        - design solution-dependent cloud infrastructure architecture (by infrastructure
          and security architect)
        - build and manage infrastructure code (by operations engineer)
        - build workflows, validate and automate deployments (by devops engineer)
        - application can then be deployed to test or production environment

* using automation/devops depends on security, application sensitivity, organization
  history, staffs skills and experience, adopted technologies, business goals and other
  factors

`back to top <#aws>`_

Cloud Models
============


Cloud Deployment Models
-----------------------
    * **Cloud-based**
        - run all, migrate existing or design and build new applications in the cloud
        - can build using low or high level services
    * **On-premises**
        - deploy and increase resources by using virtualization and resource management tools
        - also known as private cloud deployment
    * **Hybrid**
        - connect cloud-based resources to on-premises infrastructure
        - use when there are legacy applications or regulations that require to keep certain
          records on premises
    * upfront expense: physical resources that need to be invested before using
    * variable expense: only pay for computing resources consumed
    * benefits of cloud computing
        - trade upfront expense for variable expense
        - benefit from massive economies of scale (the higher economies of scale, the lower
          pay-as-you-go prices)
        - stop guessing capacity (no need to predict how much infrastructure capacity will be
          needed)
        - increase speed and agility (flexible and fast to access new resources)
        - stop spending money running and maintaining data centers (focus less on spending
          money and time managing infrastructure)
        - go global in minutes (can deploy global infrastructure)

Main Cloud Computing Models
---------------------------
    * **IaaS (Infrastructure as a Service)**
        - contains basics for cloud (networking, computers and storage space)
        - highest level of flexibility and control over resources
    * **PaaS (Platform as a Service)**
        - no need to manage underlying infrastructure
        - focus on deployment and management of applications
    * **SaaS (Software as a Service)**
        - everything is managed by service provider
        - most refer SaaS to end-user applications (e.g web-based email)
        - only think about how the software will be used

`back to top <#aws>`_

Well-Architected
================

* helps cloud architects build secure infrastructure, and is built around six pillars
* framework was created to
    - provide consistent approach to evaluating architectures
    - ensure that customers are thinking cloud natively
    - ensure that customers are thinking about the foundational areas that are often neglected
* components of framework: questions, design principles and pillars

General design principles
-------------------------
    * **traditional environment**
        - had to guess infrastructure needs
        - could not afford to test at scale
        - had a fear of change
        - could not justify experiments
        - face frozen architecture
    * **cloud environment**
        - stop guessing capacity needs
        - test systems at production scale
        - automate to make architectural experimentation easier
        - allow for evolutionary architectures
        - drive architectures using data
        - improve through game days

Operational Excellence
----------------------
    * focuses on running and monitoring systems
    * automating changes, responding to events and defining standards to manage operations
    * in traditional environment
        - manual changes, batch changes
        - rarely run game days
        - no time to learn from mistakes, stale documentation
    * in cloud environment
        - perform operations as code
        - make frequent, small, reversible changes
        - refine operations procedures frequently
        - anticipate failure and learn from all operational failures
    * organization
        - operational priorities, operating models, organizational culture
        - involve business and development teams in setting operational priorities
        - consider internal and external compliance requirements
        - consider trade-offs among priorities and make informed decisions
    * prepare
        - design telemetry, improve flow, mitigate deployment risks, understand operational
          readiness
        - provide developers with individual sandboxes and environments
        - use infrastructure as code and configuration management systems
        - increase controls as environments approach production
        - turn off when environments are not in use
    * operate
        - understand workload and operations health
        - events: observation of interest
        - incident: an event that requires a response
        - problem: an incident that either recurs or cannot currently be resolved
        - runbooks and playbooks should define what causes an issue and the process for the
          issue,and identify owners for each action
        - users should be notified when services are impacted and when return to normal
    * evolve
        - learn from experience, make improvements and share learning
        - have feedback loops
        - identify ares for improvements and prioritize and drive them where needed
        - begin on operations activities, customer experience and business and development
          teams and recognize improvement

Security
--------
    * focuses on protecting information and systems
    * integrity of data, managing users and controls to detect security events
    * in cloud environment
        - implement strong identity foundation
        - use fine-grained access controls
        - apply security at all layers
        - prepare for security events and automation
    * use AWS Accounts, Organizations, Control Tower
    * identity and access management
        - use AWS IAM and have strong sign-in mechanisms
        - centralized identity provider and secure secrets
        - define access requirements and grant least privilege
        - limit public and cross-account access and reduce permissions continuously
    * detection
        - lifecycle controls to establish baseline
        - internal auditing and automated alerting
    * infrastructure protection
        - have trust boundaries, system security configuration, OS authentication and policy
          enforcement points
        - use edge services such as CloudFront and WAF
        - divide VPC into layers using subnets and use all controls available
        - use AWS managed services to focus on securing the workload
    * data protection
        - classify data, keep people away from data and use CI/CD
        - protect data in transit and at rest
        - provide a dashboard instead of direct query of data
    * incident response
        - have right tools pre-deployed and conduct game days regularly
        - create new trusted environment to conduct investigations

Reliability
-----------
    * focuses on workloads performance and recovery from failures
    * distributed system design, recovery planning and adapting to changing requirements
    * in traditional environment
        - manual recover from failure, rare test recovery procedures
        - multiple single points of failure, need to guess capacity
    * in cloud environment
        - automatic recovery, test recovery procedures
        - scale horizontally, stop guessing capacity, manage change in automation
    * foundations
        - scope extends beyond single workload or project
        - have sufficient network topology and understand service quota
    * workload architecture
        - use microservices or service-oriented architecture (SOA)
        - design to prevent and mitigate failures
    * change management
        - monitor behavior, automate response and review
        - auto scale to meet demands
        - integrate testing as part of deployment and use immutable infrastructure
    * failure management
        - automate, analyze, backup data and recover
        - use multiple AZs or Regions as needed
        - bulkhead architecture (partitions or shards: bulkhead for data, cells: bulkhead for
          services)
        - have periodic recovery testing and reviews
        - test function and performance with playbooks and use chaos engineering

Performance Efficiency
----------------------
    * focuses on structured allocation of computing resources
    * selecting resource types, monitoring performance and maintaining efficiency
    * in traditional environment
        - use of same technology and local
        - use servers with people to manage and hard to experiment
    * in cloud environment
        - try new technologies, go global in minutes
        - use serverless and experiment more often
        - consider mechanical sympathy
    * selection
        - select appropriate resource types
        - benchmark and load test and monitor performance
    * review
        - review selections as new resources are innovated
        - experiment with features in low-risk way
    * monitoring
        - use alerts and automation to work around performance issues (CloudWatch, Kinesis,
          SQS, Lambda)
    * trade-offs
        - optimize resource position, add read-only replicas and measure impact
        - use CDN, database caching, proactive monitoring and notification

Cost Optimization
-----------------
    * focuses on avoiding unnecessary costs
    * controlling fund allocation, selecting right resources and scaling without
      overspending
    * in traditional environment
        - centralize cost, labor to maintain servers
        - pay upfront, no benefit from economies of scale
    * in cloud environment
        - adopt consumption model, measure overall efficiency
    * practice cloud financial management
        - stop spending money on heavy lifting, analyze and attribute expenditure
    * expenditure and usage awareness
        - tag resources, track applications
        - monitor usage and spend (Cost Explorer, partner tools)
    * cost-effective resources
        - on-demand, savings plans, reserved and spot instances
        - analyze available services and take advantage
        - consider application-level services and automation
    * manage demand and supply resources
        - resource provisioning, monitoring tools and benchmark
        - manage demand with queue or buffer and supply through auto scaling (demand based or
          time based)
    * optimize over time
        - decommission resources or services
        - stay up to date on new services and features

Sustainability
--------------
    * focuses on minimizing environmental impacts of running workloads
    * shared responsibility, understanding impact and maximize utilization and minimize
      required resources

* framework includes domain-specific lenses and AWS Well-Architected Tool, a tool to
  evaluate workloads

* terms
    * component: code, configuration and resources
    * workload: a set of components that deliver business value
    * architecture: how components work together in a workload
    * milestones: key changes in architecture as it evolves
    * technology portfolio: collection of workloads that are required for the business

* trade-offs will be made between pillars based on the business context
* security and operational excellence are generally not traded-off against others

* common uses of framework
    *  learn how to build cloud-native architecture
    * build a backlog
    * use as a gating mechanism before launch
    * compare maturity of different teams
    * perform due diligence for acquisitions

Well-Architected Tool
---------------------
    * main features
        - define workloads and perform reviews
        - locate helpful resources, save milestones
        - use dashboard, generate pdf reports
        - assign priorities to pillars, create improvement plan

`back to top <#aws>`_

AWS Global Infrastructure
=========================

* built closest to adhere business demands

Regions
-------
    * geographical area that contains AWS resources
    * each Region has multiple data centers
    * each Region can be connected to another through high speed fiber network
    * each Region is isolated from every other
    * data can only move from Region to Region if permission is explicitly granted
    * **choosing a Region**
        1. compliance requirements
        2. proximity to customers
        3. feature availability
        4. pricing

Availability Zones (AZs)
------------------------
    * each AZ is one or more discrete data centers within a Region
    * each Region consists of isolated and physically separated AZs
    * high availability and disaster recovery
    * run services across at least two AZs in a Region

* may of the AWS services run at the Region level, running synchronously across multiple AZs


Edge locations
--------------
    * CDNs (Content Delivery Networks): caching copies of data closer to the customers
    * separate from Regions
    * can push content from inside a Region to collection of Edge locations
    * cache a copy locally at an edge location

everything is an API call
-------------------------
    * invoke APIs to configure and manages AWS resources
    * **AWS Management Console**
        - web-based to manage resources visually
        - to test environments, view bills and monitoring
    * **AWS CLI**
        - make API calls using terminal
        - actions can be scripted and repeated
        - less human error
    * **AWS SDKs**
        - interact with AWS resources through various programming languages
        - without using low-level APIs

`back to top <#aws>`_

Pricing & Support
=================


Free Tier
---------
    * **Always free**
        - Lambda allows for 1 million free invocations and up to 3.2 million seconds
          of compute time per month
        - DynamoDB allows 25GB of free storage per month
    * **12 months free**
        - following the initial sign up
        - S3 up to 5GB of standard storage
        - thresholds for monthly hours of EC2 compute time
        - CloudFront data transfer out
    * **Trials**
        - Lightsail offers 1 month trial of up to 750 hours of usage
        - Inspector offers 90-day free trial

Pricing
-------
    * pay for what you use
        - pay for exactly the amount of resources used without long-term contracts
    * pay less when you reserve
        - get discounts with reservations compared to On-Demand pricing
    * pay less with volume-based discounts when you use more
        - per-unit cost gets lower with increased usage
    * Lambda
        - charged based on number of re*uests for functions and time taken for them to run
        - can save cost by signing up for Compute Savings Plans, consistent usage over 1 or 3
          year term
    * EC2
        - charge based on compute time while instances are running
        - can significantly reduce costs by using Spot Instances
    * S3
        - storage: charged based on objects' sizes, storage classes and stored duration of
          each object
        - requests and data retrievals: pay for requests made to S3 objects and buckets
        - data transfer
           + no cost to transfer data between S3 buckets or from S3 to other services within
             same Region
           + no cost to transfer into S3 from the internet or out to CloudFront
           + no cost to transfer out to EC2 instance in the same Region
        - management and replication: pay for storage management features enabled (Inventory,
          analytics, object tagging)

Pricing Calculator
------------------
    * explore services and create estimate for the cost of use case
    * organize estimates by groups
    * can share an estimate with others through generated link

Billing Dashboard
-----------------
    * pay bills, monitor usage and analyze costs
    * compare current and previous month balance and get a forecast
    * view Free Tier usage by service
    * purchase and manage Savings Plans
    * publish Cost and Usage Reports

Consolidated Billing
--------------------
    * free [AWS Organizations](#organizations) feature for single billing of multiple accounts
    * can get bulk discount pricing, share Savings Plans and Reserved Instances across all
      accounts in the organization
    * default maximum accounts allowed for an organization is 4
    * can contact AWS Support to increase quota

Basic free support
------------------
    * 24/7 customer service, Documentations, Whitepapers, Support forums
    * [AWS Trusted Advisor](#trusted-advisor), AWS Health Dashboard

Developer support
-----------------
    * Basic support, email access to customer support

Business support
----------------
    * Basic and Developer support
    * [Trusted Advisor](#trusted-advisor) provides full set of checks
    * direct phone access to cloud support engineers
    * access to infrastructure event management

Enterprise support
------------------
    * Basic, Developer and Business support
    * 15-minute SLA for critical workloads
    * dedicated TAM (Technical Account Manager), who provides expertise across full range
      of services

EDP (Enterprise Discount Program)
---------------------------------
    * for large enterprises with significant AWS consumption spend
    * get discounted pricing for enterprise level consumption commitments

`back to top <#aws>`_

CAF
===

* to enable quick and smooth migration to AWS, focuses on six perspectives
* business, people and governance perspectives focus on business capabilities
* platform, security and operations perspectives focus on technical capabilities
    * **Business**
        - to create strong business case for cloud adoption and prioritize cloud adoption
          initiatives
        - ensure business strategies and goals align with IT
        - roles: business managers, finance managers, budget owners, strategy stakeholders
    * **People**
        - to evaluate organizational structures and roles, new skill and process requirements,
          and identify gaps
        - prioritize training, staffing and organizational changes
        - roles: HR, staffing, people managers
    * **Governance**
        - to understand how to update the staff skills and processes to align IT strategy with
          business strategy
        - manage and measure cloud investments to evaluate business outcomes
        - roles: CIO, program managers, enterprise architects, business analysts, portfolio
          managers
    * **Platform**
        - to understand and communicate structure of IT systems and their relationships
        - includes principles and patterns for implementing new solutions on the cloud and
          migrating on-premises workloads to the cloud
        - describe the architecture of the target state environment in detail
        - roles: CTO, IT managers, solutions architect
    * **Security**
        - to structure the selection and implementation of security controls for visibility,
          auditability, control and agility that meet the organization's needs
        - roles: CISO, IT security managers, IT security analysts
    * **Operations**
        - to enable, run, use, operate and recover IT workloads to the level agreed upon with
          business stakeholders
        - define current operating procedures and identify the process changes and training
          needed to implement successful cloud adoption
        - roles: IT operation managers, IT support managers
* CAF Action Plan
    * each perspective is used to uncover gaps in skills and processes, which are recorded as inputs
    * inputs are used to create CAF Action Plan
    * helps guide organization for cloud migration

`back to top <#aws>`_

6 Rs
====


Rehosting
---------
    * lift and shift, not changing anything
    * move applications as is into AWS, may not be optimized at first
    * for large legacy migration

Replatforming
-------------
    * lift, tinker and shift
    * make few optimizations, but not changing core architecture of the application

Refactoring/re-architecting
---------------------------
    * changing application architecture and develop by using cloud-native features
    * driven by strong business needs to add features, scale or performance that is hard
      to achieve in existing environment

Repurchasing
------------
    * moving from traditional license to a software-as-a-service model
    * abandon legacy vendors and move to cloud-native ones

Retaining
---------
    * keeping applications critical for the business
    * include applications that require major refactoring before migrated

Retiring
--------
    * removing applications that are no longer needed
    * to save cost and effort

`back to top <#aws>`_

Cloud Acquisition
=================


Procurement
-----------
    * buying cloud technologies is paying for access to standardized compute, storage and other
      IT services that run in a cloud service provider (CSP) data centers
    * different from traditional on-premises hardware as only pay for the resources used
    * reevaluate existing procurement strategies
    * create flexible acquisition process
    * successful cloud procurement strategies extract full benefits of the cloud

Legal
-----
    * engage CSPs early to get best fit and resolve differences
    * avoid traditional IT terms and conditions as the basis for contract
    * use the CSP's terms as possible to avoid misalignment
    * recognize different terms and conditions among CSPs, cloud-managed service providers
      and resellers

Security
--------
    * research existing industry best practice
    * use third-party auditing to evaluate CSPs to prevent overly burdensome processes
    * ask whether CSP is certified for specific third-party accreditation

Governance
----------
    * shared responsibility between CSP and customers
    * ensure that the contract can be used effectively or operationalized

Finance
-------
    * think beyond fixed-price contracting
    * account for fluctuating demand with pay-as-you-go model
    * cloud pricing fluctuates based on market pricing
    * allow for flexibility in procurement strategy
    * evaluate different CSPs based on publicly available pricing and tools

Compliance
----------
    * compliance requirements differ based on geographical location
    * ask CSPs and Partners about how they comply and what tools are supported

* engage all **stakeholders** early and often
    * help them understand how cloud will affect their area
    * understand how they will need to change and adapt internal skills and processes
    * security professionals
        - consider how to get security assurance through third-party audits
        - consider skills and capability needed
    * HR
        - think about training or retraining staff to use new systems
    * finance
        - consider internal spend controls and use tools to predict, manage and control costs
    * program managers
        - think about shared responsibility model
        - think about ways to deploy new programs to exploit visibility and control in cloud
          environment
* buying the cloud
    * Direct
        - purchase as a commercial item without labor hours
        - accept the [AWS Agreement](https://aws.amazon.com/agreement/) (online customer/AWS Enterprise) with standard terms and conditions
    * Indirect
        - purchase from CSP partner/reseller
        - negotiate customized agreement with the CSP

* separate technology provided by CSP from hands-on services and labor
* CSPs operate at massive one-to-many scale to offer services
* key aspects of procurement: pricing, security, governance, terms & conditions
* data sovereignty: ownership over data
* data residency: requirement that all processed and stored content remain within specific
  country's borders

AWS Partner Network (APN)
-------------------------
    * helps identify and choose high-quality AWS Partners with deep AWS expertise
    * consulting partners: provide customers with professional services
    * technology partners: provide customers with software solutions
    * AWS offers Partners solution provider program, managed service provider (MSP) program,
      competency program, service delivery program

* AWS programs
    * AWS re/Start
        - 12-week program for disadvantaged people and members of armed forces community
    * AWS Educate
        - free cloud curriculum content for recognized educational institutions
    * AWS Startup Day
        - free full-day events to deliver education, networking opportunities and experiences
          of start-up founders
    * AWS Lofts
        - a place where start-ups and developers can meet at no cost
    * AWS Activate
        - to provide start-ups with resources to test and grow
        - can be applied direct or through a participating accelerator, incubator, seed or VC
          fund or other start-up enabling organizations
        - 3 tiers providing up to $100,000 of free platform credits, training and support

* can query AWS prices with The Price List Service API (Query API, JSON) and AWS Price List API
  (Bulk API, HTML)
* can subscribe to Amazon SNS to get alerts when prices change
* prices change periodically when AWS cuts prices, new instance types are launched or new
  services are introduced
* different ways for cloud migration
    * Agile approach
        - to accelerate cloud migrations at scale
        - epics: work that can be broken down into specific tasks
        - use epics for defined workstreams in the readiness and planning phase
        - structure work in the form of epics and stories to be able to respond to change
        - produce well-prioritized backlog, report progress
        - build migration roadmap to tie business stakeholder needs to technology initiatives
    * Sprints
        - for migration planning and implementation
        - align, enable and mobilize workforce and resources
        - start small, create early success and build to full migration
        - capture outputs, best practices and lessons learned
        - create blueprints of agile delivery for scaling migrations based on Wave 1 ( the
          initial wave of migrations with 10 to 30 applications)
    * can scale teams to support initial wave of migrations after agile and sprints to build
      migration factory process
    * AWS Incentives
        - AWS Cloud Adoption Framework (AWS CAF), AWS Professional Services (ProServe)
        - AWS Cloud Adoption Readiness Tool (CART) helps customers develop efficient and
          effective plans
    * AWS Migration Partners
        - can help through every stage of migration
        - accelerate results by providing personnel, tools and education

`back to top <#aws>`_

Application Integration
=======================

* `SNS`_, `SQS`_

SNS
---
    * Simple Notification Service
    * send notifications to end users in publish/subscribe model using mobile
      push, SMS and email
    * SNS topic: channel for messages to be delivered
    * sending a message to a topic will go out to all subscribers of that channel
    * subscribers can also be endpoints such as SQS queues, Lambda functions, HTTPS or
      HTTP web hooks

SQS
---
    * Simple Queue Service
    * send, store and receive messages between software components at any volume
    * payload: data within the message which is protected until delivery
    * messages are sent into a queue and retrieved from it and deleted after processed

`back to top <#aws>`_

Compute
=======

*  [EC2](#ec2), [ELB](#elb), [Lambda](#lambda), [Elastic Beanstalk](#elastic-beanstalk), [Outposts](#outposts)

EC2 (Elastic Compute Cloud)
---------------------------
    * Compute as a Service Model
    * each instance is a virtual machine
    * multitenancy: sharing underlying hardware between VMs
    * hyper visor is responsible for multitenancy and isolating the VMs
    * one instance is not aware of another on the same host
    * pay only for the compute time when an instance is running
    * can choose OS and specific hardware configuration
    * flexible and resizable/vertical scaling, and auto-scalable
    * **Instance types**
        - **General purpose**
           + balance of compute, memory and networking resources
        - **Compute optimized**
           + compute intensive tasks
           + e.g gaming servers, HPC, scientific modeling, batch processing workloads
        - **Memory optimized**
           + for workloads to process large datasets in memory
           + e.g high-performance database, perform real-time processing of unstructured data
        - **Accelerated computing**
           + use hardware accelerators or coprocessors
           + e.g floating point calculations, graphics processing, data pattern matching
        - **Storage optimized**
           + for high, sequential read and write access to large datasets on local storage
           + suitable for high IOPS requirement
           + e.g distributed file systems, data warehousing apps, OLTP systems
    * **Pricing**
        - **On-Demand**
           + for short-term, irregular, uninterruptible workloads
           + instances run continuously until stopped
           + pay only for compute time used
           + e.g developing and testing apps, running apps that have unpredictable usage
             patterns
        - **Savings Plans**
           + commit consistent amount of compute usage for 1 or 3-year term
           + measured in $/hour, more flexible than Reserved Instances
           + usage beyond commitment is charged at regular On-Demand rates
           + payment options: all upfront, partial upfront or no upfront
           + Compute savings plans: up to 66% over On-Demand cost
           + EC2 Instance savings plans: up to 72% for commitment to usage of individual
             instance families in a Region
           + SageMaker savings plans: 64% savings
        - **Reserved**
           + Standard (up to 72%) or Convertible reserved (up to 66%) for 1 or 3-year term
           + Scheduled reserved for recurring schedule can pay by $/hour
           + no, partial or all upfront
           + charged with On-Demand rates at the end of term until terminated or purchase new
             Reserved instance that matches
        - **Spot**
           + for workloads that can have interruptions
           + use unused/spare EC2 capacity and savings up to 90% off of On-Demand prices
           + Spot request is unsuccessful until capacity is available
           + AWS can reclaim if needed giving a two-minute warning to save state
           + e.g batch workloads
        - **Dedicated Hosts**
           + physical servers with EC2 instances
           + can use existing software licenses
           + On-Demand Dedicated Hosts (hourly) and Dedicated Hosts Reservations (up to 70%
             off the On-Demand price)
           + most expensive type
    * **Security Groups**
        - for instance level access
        - every instance launched comes with security group
        - does not allow any traffic into the instance by default
        - check traffic on the way in but all traffics are allowed to go out of the instance

ELB (Elastic Load Balancing)
----------------------------
    * auto distributes incoming traffic across multiple resources
    * acts as a single point of contact for all traffic to Auto Scaling group
    * EC2 instances can be added or removed without disruption
    * ELB and Auto Scaling can work together even though they are separate services
    * regional construct: run at the Region level rather than on individual EC2 instances,
      therefore, is highly available
    * can also be used for internal traffic in decoupled architecture
    * e.g front-end and back-end stay separated with ELB between, distributing all
      all traffics to back-end instances

* tightly coupled architecture (Monolithic)
    * applications communicate directly
    * if single component fails or changes, it may cause issues to entire application
* loosely coupled architecture (Microservices)
    * a buffer is between applications to communicate
    * the buffer holds the message until it is processed
    * messages can still be sent to buffer even if the other application is down
    * prevents entire application from failing

Lambda
------
    * do not need to provision servers
    * flexibility to auto scale serverless applications by modifying units of consumptions
      such as throughput and memory
    * upload code to Lambda, set code to trigger from an event source, code runs only when
      triggered, pay only for compute time used
    * default metrics provided on CloudWatch
        - Invocation metrics, Performance metrics, Concurrency metrics
    * maximum 10GB storage and 15 minute execution for running a lambda function

Elastic Beanstalk
-----------------
    * helps provision EC2-based environments
    * builds environment from provided application code and configurations
    * makes it easy to save environment configurations to be deployed again
    * focus on business application, not the infrastructure

Outposts
--------
    * fully operational mini Region on-premises
    * owned and operated by AWS
    * isolated within building with AWS services

`back to top <#aws>`_

Containers
==========

* `ECS`_, `EKS`_, `Fargate`_

ECS
---
    * Elastic Container Service
    * containers: package application's code and dependencies into single object
    * run and scale containerized applications
    * supports Docker containers (Community and Enterprise Editions)
    * can use API calls to launch and stop applications

EKS
---
    * Elastic Kubernetes Service
    * deploy and manage containerized applications at scale
    * new features from K8s community can be easily applied to applications on EKS

* both ECS and EKS can run on top of EC2, both are container orchestration tools

Fargate
-------
    * serverless compute engine for containers
    * works with both ECS and EKS
    * pay only for resources used to run containers

`back to top <#aws>`_

Cost Management
===============

* `Budgets`_, `Cost Explorer`_, `Marketplace`_

Budgets
-------
    * set custom budgets to plan service usage, costs and instance reservations
    * information is updated three times a day
    * set alerts if usage exceeds the limit
    * send emails up to 10 addresses, integrate with AWS Chatbot to receive alerts through
      Slack and Amazon Chime
    * can compare current/forecasted vs budgeted

Cost Explorer
-------------
    * console based service to visualize and analyze cost usage
    * gives 12 months of data to track
    * has default report of costs and usage for top five cost-accruing services
    * can create custom reports

Marketplace
-----------
    * can choose third-party software with different payment options
    * can use one-click deployment
    * most vendors allow already owned licenses to use for AWS deployment and offer
      pay-as-you-go pricing
    * many vendors offer free trials or quick start plans
    * enterprise focused features
        - custom terms and pricing
        - private marketplace
        - integration into procurement systems
        - cost management tools

`back to top <#aws>`_

Database
========

* `RDS`_, `DynamoDB`_, `Redshift`_, `DMS`_, `DocumentDB`_, `Neptune`_, `Managed Blockchain`_, `QLDB`_, `ElastiCache`_
* relational databases
    * data is stored in a way that relates it to other pieces of data
    * uses SQL to store and query data
* nonrelational databases
    * use key-value (item-attribute) pairs rather than rows and columns
    * not every item in the table has to have the same attributes

RDS
---
    * Relational Database Service
    * enables to run relational databases in the AWS Cloud
    * supported database engines: Aurora, PostgreSQL, MySQL, MariaDB, Oracle, Microsoft SQL
    * AWS manages automated patching, backups, redundancy, failover, disaster recovery
    * offer encryption at rest and encryption in transit
    * **Aurora**
        - fully managed enterprise-class relational database engine
        - compatible with MySQL and PostgreSQL
        - 1/10th the cost of commercial databases by reducing unnecessary I/O
        - replicates six copies of data and up to 15 read replicas
        - continuous backup to S3 and point-in-time recovery
        - for workloads that require high availability

DynamoDB
--------
    * serverless NoSQL database
    * create tables to store data
    * data is organized into items which have attributes
    * stores data across multiple AZs and mirrors data across multiple drives
    * has single-digit millisecond response time
    * **DAX (DynamoDB Accelerator)**
        - native caching layer
        - improve response times from single-digit milliseconds to microseconds

Redshift
--------
    * data warehousing as a service, ideal for business intelligence (BI) workloads and data
      analytics
    * can collect data from many sources
    * can directly run single SQL query against exabytes of unstructured data
    * up to 10 times higher performance than traditional databases

DMS
---
    * Database Migration Service
    * migrate existing databases (source) onto AWS (target)
    * source database remains fully operational during the migration
    * source and target don't have to be of same type
    * homogenous migration
        - from MySQL to RDS MySQL
        - schema, data types and database code are compatible between source and target
    * heterogeneous migration
        - schema, data types and database code are different between source and target
        - 2-step process
        - first use AWS Schema Conversion Tool and use DMS to migrate
    * useful for development and test database migrations, database consolidation, continuous
      database replication

DocumentDB
----------
    * for content management, catalogs, user profiles
    * supports MongoDB workloads

Neptune
-------
    * graph database
    * for social networking, recommendation engines and fraud detection

Managed Blockchain
------------------
    * to create and manage blockchain networks with open-source frameworks

QLDB
----
    * Quantum Ledger Database
    * ledger database service
    * immutable system of record

ElastiCache
-----------
    * provide caching layers without worrying about underlying
    * supports Memcached and Redis

`back to top <#aws>`_

Artificial Intelligence
=======================

* `A2I`_, `Comprehend`_, `DeepRacer`_, `Fraud Detector`_, `Lex`_, `SageMaker`_, `Textract`_, `Transcribe`_

A2I
---
    * Augmented AI
    * provides built-in human review workflows for common machine learning use cases
    * can also create own workflows for machine learning models built on SageMaker or others
    * e.g content moderation, text extraction from documents

Comprehend
----------
    * discover patterns in text

DeepRacer
---------
    * autonomous 1/18 scale race car to test reinforcement learning models
    * train the car to drive around the track as fast as possible
    * use PPO and SAC to train stochastic policies, have over 20 parameters for use
    * **Reward Function**
        - code that uses the input parameters for calculations on input and output a reward
        - in Python ``def reward_function(params)``, [Available Parameters](https://docs.aws.amazon.com/deepracer/latest/developerguide/deepracer-reward-function-input.html)
        - Example Parameters: ``track_width``, ``distance_from_center``, ``steering_angle``
        - more complex reward function does not mean better results
    * **Components**
        - onboard computer with Wi-Fi and Intel Atom processor running Ubuntu Linux
        - can equip up to two single-lens cameras and LIDAR sensor
    * need to achieve efficient inference with limited resources of the car, by making sure the
      machine learning model is appropriate for the inference engine
    * **OpenVINO**
        - Open Visual Inferencing and Neural Network Optimiser
        - Intel developer toolkit for machine learning inference used with DeepRacer
        - helps to improve inference performance of models on Intel hardware
        - contain Deep Learning Deployment Toolkit with model optimiser, Inference engine, and
          pre-trained models
    * **Optimisation**
        - model optimiser generate Intermediate Representation (IR) files for OpenVINO to use
        - optimisation is done automatically when the model is exported from DeepRacer console
        - can use benchmarking and accuracy checker tools to refine and optimise
    * **Post-Training Optimisation Toolkit**
        - POT, OpenVINO tool to make the model more efficient
        - can reduce precision by sacrificing some accuracy to get extra performance
        - some Intel processors has accelerators to turbo charge performance of lower
          precision models

Fraud Detector
--------------
    * identify potentially fraudulent online activities

Lex
---
    * build voice and text chatbots

SageMaker
---------
    * remove difficult work from the process to build, train and deploy ML models quickly

Textract
--------
    * automatically extracts text and data from scanned documents

Transcribe
----------
    * convert speech to text

`back to top <#aws>`_

Management & Governance
=======================

* `Auto Scaling`_, `CloudFormation`_, `CloudTrail`_, `CloudWatch`_, `Config`_
* `Organizations`_, `Systems Manager`_, `Trusted Advisor`_

Auto Scaling
------------
    * begin with only resources needed and auto respond to changing demand by scaling
      out or in
    * **dynamic scaling**
        - responds to changing demand
    * **predictive scaling**
        - auto schedule number of instances based on predicted demand
    * can use both methods together to scale faster
    * **auto scaling group**
        - minimum capacity: launched immediately after the group is created
        - desired capacity: can be set or defaults to minimum capacity if not set
        - maximum capacity: max when scaling out
    * vertical scaling: resizing an instance to add more power
    * horizontal scaling: launching new instances (EC2 Auto Scaling)
    * uses public-key cryptography to encrypt and decrypt login information

CloudFormation
--------------
    * IaC tool
    * provisions and configures resources using a template file, JSON or YAML format
    * no additional charge, only pay for resources created
    * support many resources, not just EC2-based
    * *CloudFormation Engine* calls the APIs and build everything
    * **CloudFormation Designer**
        - graphic tool for creating, viewing, and modifying AWS CloudFormation templates
        - make template using drag-and-drop
        - shows relationship between template resources
    * allows duplication of environments to recreate and analyze errors

CloudTrail
----------
    * track user activity and API usage across AWS Regions and accounts
    * every request gets logged in the CloudTrail engine
    * monitor activity events and generate audit reports
    * detect unauthorized access in CloudTrail Events and respond
    * monitor API usage history using ML models
    * logs can be stored in S3 buckets
    * events are updated within 15 minutes after an API call
    * CloudTrail Insights: auto detect unusual API activities

CloudWatch
----------
    * monitoring and observability service
    * collects data in the from of logs, metrics and events to monitor, respond and optimize
      across all resources on single platform
    * can be used to detect anomalous behavior, set alarms and take automated actions
    * reduce MTTR (mean time to resolution) and improve TCO (total cost of ownership)

Config
------
    * to record resource configuration changes and evaluate against rules
    * configure rules coupled with automated remediation
    * automate response to misconfigurations

Organizations
-------------
    * central location to manage multiple accounts
    * consolidated billing and hierarchical groupings of accounts
    * can group accounts into organizational units (OUs)
    * services and API actions access control with SCPs (Service control policies) to
      organization root, individual member accounts or OUs

Systems Manager
---------------
    * collection of features that enable IT Operations
    * no additional charge, only pay for resources created
    * pre-requisites for resources to be managed with Systems Manager
        - must use supported OS (Windows, Amazon Linux, Ubuntu, RHEL, CentOS)
        - SSM Agent must be installed (Windows requires PowerShell 3.0 or later)
           + installed by default on Amazon Linux base AMIs dated 2017.09 and later
           + Windows Server 2016 instances
           + instances created from Windows Server 2003-2012 R2 AMIs from November 2016 or
             later
        - EC2 instances must have outbound internet access
        - access Systems Manager in supported region
        - requires IAM roles for instances that will process commands and for users executing
          commands
    * **Inventory**
        - collect operating system (OS), application, and instance metadata from EC2 instances
          and on-premises servers or virtual machines (VMs) in hybrid environment
        - query metadata to quickly understand which instances are running the software and
          configurations required by software policy, and which instances need to be updated.
    * **State Manager**
        - association
           + defines the state to apply to a set of targets
           + includes document that defines the state, targets, schedule and optional runtime
             parameters
    * **Compliance**
        - scan the fleet of managed instances for patch compliance and configuration
          inconsistencies
        - collect and aggregate data from multiple AWS accounts and Regions
        - displays compliance data about Patch Manager patching and State Manager associations
          by default
        - can also port data to Amazon Athena and Amazon QuickSight to generate fleet-wide
          reports
    * **Patch Manager**
        - automates process of patching managed instances with security related updates
        - can also install non-security updates fro Linux-based instances
        - patch fleets of EC2 instances
        - scan instances to see only a report of missing patches or auto install missing ones
        - target instances individually or in large groups by using Amazon EC2 tags
        - always test patches thoroughly before deploying to production environments
        - patch baselines
           + include rules for auto-approving patches within days of release
        - integrate with IAM, CloudTrail and CloudWatch Events
        - patch group
           + can create by using EC2 tags
           + groups must be defined with the tag key, which is case sensitive
           + an instance can only be in one patch group
    * **Documents**
        - defines the actions that Systems Manager performs on managed instances
        - us JSON or YAML notation
    * **Run Command**
        - remotely and securely manage configuration of managed instances
        - enables to automate common administrative tasks
    * **Maintenance Windows**
        - define a schedule to perform disruptive actions on instances
        - each Window has a schedule, duration, set of registered targets and tasks
        - can install apps, update patches, execute PowerShell commands and Linux shell
          scripts by using Run Command, build AMIs, execute Lambda functions, run AWS Step
          Function state machines
    * **OpsCenter**
        - can be created to track events and understand the current status of an event
        - can help answer questions such as - level of severity, affected resources, status of
          the event and similar events

Trusted Advisor
---------------
    * auto evaluate resources against five pillars (cost optimization, performance, security,
      fault tolerance, service limits)
    * has free checks and others based on support plan
    * green: no problems, orange: recommended investigations, red: recommended actions

`back to top <#aws>`_

Migration & Transfer
====================

* `Snow Family`_

Snow Family
-----------
    * physical devices for faster migration of exabytes of data into and out of AWS
    * hardware and software are cryptographically signed and data stored is auto encrypted
    * can use KMS to manage keys
    * **Snowcone**
        - 2 CPUs, 4 GB of memory and 8 TB of data and contains edge computing (EC2 and IoT Greengrass)
        - order via Management Console, copy data and ship it back to AWS
    * **Snowball**
        - **edge compute optimized**
           + for machine learning, full motion video analysis, analytics and local computing
           + 42 TB HDD for S3 compatible or EBS compatible, 7.68 TB of NVMe SSD for EBS
             compatible
           + 52 vCPUs, 208 GiB of memory and optional NVIDIA Tesla V100 GPU
           + run EC2 sbe-c and sbe-g instances (equal to C5, M5a, G3 and P3)
        - **edge storage optimized**
           + for large-scale data migrations
           + 80 TB of HDD for block volumes and S3 compatible, 1 TB of SATA SSD for block
             volumes
           + 40 vCPUs and 80 GiB of memory to support EC2 sbe1 instances (equal to C5)
        - fit into existing server racks
        - can run Lambda functions, EC2-compatible AMI or IoT Greengrass
        - usually for remote locations
    * **Snowmobile**
        - up to 100 petabytes of data per snowmobile
        - ideal for largest migrations and data center shutdowns
        - 45-foot long ruggedized shipping container, pulled by semi trailer truck

`back to top <#aws>`_

Networking & Content Delivery
=============================

* `CloudFront`_, `Direct Connect`_, `Route 53`_, `VPC`_, `Global Accelerator`_

CloudFront
----------
    * deliver data to customers around the world with low latency
    * uses *Edge locations*
    * retrieves data from origin, the cache in edge location and delivers to customers

Direct Connect
--------------
    * private, dedicated, low latency physical fiber connection from on-premises to AWS
    * need to work with Direct Connect Partner to establish the connection

Route 53
--------
    * DNS with low latency
    * uses *Edge locations*
    * can direct traffic to different endpoints using routing policies (latency-based,
      geolocation, geoproximity, weighted round robin)
    * can register domain names

VPC
---
    * Virtual Private Cloud, to create logically isolated networks
    * manage resources within ip ranges of the network
        - recommended range (RFC1918): 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16
    * usually for back-end services like databases or application servers
    * **Subnets**
        - allow to group resources and make them public or private
    * can control what traffic gets into VPCs
    * **Route Tables**
        - rules for which packets go where
        - VPC has default route table
        - different subnets can be assigned different route tables
    * **IGW (Internet Gateway)**
        - to allow public traffic into public subnet of VPC
    * **NAT Gateway**
        - allow data from private subnet of VPC to go outside the Internet, outbound internet
        - one way gateway, only responses of requests from AWS resources are allowed
        - requests from the Internet are not allowed to go through
    * **Security Groups**
        - works at AWS network-level, distributed firewall
        - all ports are blocked by default
        - modify the group to allow specific types of traffic into instances
        - instance level and support allow rules only
        - check traffic on the way in but all traffics are allowed to go out
        - *stateful*: recognizes the packet from before and will not check again if it returns
    * **Network ACLs (Access Control Lists)**
        - virtual firewall to check packets having permissions or not to enter or leave the
          subnet
        - subnet level, support allow and deny rules
        - doesn't evaluate if a packet can reach a specific internal instance or not
        - allow all inbound and outbound traffics by default
        - *stateless*: does not recognizes the packet from before and will check again if it
          returns
        - should only have short and simple rules, use security groups for complex ones
    * **Flow Logs**
        - works at VPC, subnet or instance level
        - output can be directed to S3 bucket or CloudWatch
        - useful for visibility, troubleshoot and analyze traffic flow
    * **DNS**
        - DNS services are enabled by default
        - provide DNS resolution and able to assign DNS names
    * **VPC peering**
        - connecting two VPCs for communication
        - only one-to-one relationship between connection
        - can be in different regions or accounts, CIDR ranges must no overlap
        - need to update route table for traffic to flow
        - maximum 125 peerings, cost per data transfer
    * **Transit Gateway**
        - distributed managed routing service deployed in a region
        - has its own routing table for connected VPCs
        - VPCs can be in different accounts
        - VPC and Transit Gateway must be in same region
        - maximum 5000 attachments, 50Gbps per VPC attachment
        - cost per data transfer and attachment
    * **Virtual Private Gateway**
        - to allow traffic from approved network
        - allows to create VPN connection from other private network, on-premises internal
          resources, to VPC
    * one VPC can have multiple types of gateways attached
    * **VPC Sharing**
        - subnets can be shared between multiple accounts
        - resources can be created inside the VPC from different account
        - use fewer IPv4 CIDRs, no VPC peering required, VPC management can be centralized
    * **VPC endpoints**
        - Gateway endpoints: only for S3 and DynamoDB, for private connection without using
          public IPs
        - Interface endpoints: for all services inside VPC

Global Accelerator
------------------
    * network layer service, deployed in front of Internet facing apps to optimize performance
    * can cover any type of application running on TCP or UDP

`back to top <#aws>`_

Security & Compliance
=====================

* `Shared Responsibility Model`_, `IAM`_, `Artifact`_, `Cognito`_, `GuardDuty`_, `Inspector`_, `KMS`_, `WAF & Shield`_

Shared Responsibility Model
---------------------------
    * shared responsibility between AWS and customer
    * Security of the cloud (AWS)
        - protect the infrastructure that runs all the services in the AWS
        - hardware, software, networking and all facilities
        - AWS manages the host OS and virtualization layer down to the physical security
    * Security in the cloud (customer)
        - determined by services that customer selects
        - for EC2, customer has to perform security configuration and management tasks,
          management of the guest OS and software installed and AWS-provided firewall
        - for abstracted services, S3 and DynamoDB, customers are responsible for managing
          data, encrypting, classifying assets and using IAM tools to apply permissions
    * **inherited controls**
        - controls customers inherit from AWS
        - physical and environmental controls
    * **shared controls**
        - both infrastructure and customer layers, but in separate contexts or perspectives
        - AWS provides requirements for infrastructure and customers must provide own control
          implementation to the use of services (guest OS, databases, applications)
        - patch management, configuration management
        - awareness & training: AWS trains AWS employees, customers train their own employees
    * **customer specific**
        - customer's responsibility based on the application deployed within AWS services
        - service and communications protection
        - zone security to route data within specific security environment
    * applying the Model in practice
        - determine external and internal security and compliance requirements (NIST CSF, ISO)
        - consider employing AWS CAF and Well-Architected practices
        - review security and configuration options
        - evaluate AWS security, identity and compliance services
        - review third-party documents
        - explore solutions in the AWS Marketplace
        - explore AWS security competency partners

IAM
---
    * Identity and Access Management, authentication and authorization as a service
    * can specify who or what can access services and resources in AWS
    * create permissions based on department, job role or team name
    * manage per-account or use IAM Identity Center to provide multi-account access
    * IAM policies
        - json documents which can be attached to users or groups
    * IAM roles
        - access to temporary permissions, no username or password
        - when an identity assumes a role, it abandons all of the previous permissions and
          assumes the permissions of that role
        - can map corporate identities to IAM roles
    * recommended to create IAM user with admin permissions to use for everyday tasks
    * **Root user**
        - account owner, created when AWS account is created
        - **credentials**
           + full access to all resources in the account
           + cannot use IAM policies to deny root user access to resources
           + can only use AWS Organizations SCP to limit permissions of the root user
           + only root user can close the account
    * **IAM user**
        - Identity and Access Management user
        - created by root user or IAM admin
        - least privilege principle: user is granted access only to what they need
        - no permissions associated by default
        - **credentials**
           + securely control access to AWS services and resources for users in the account
           + can create unique credentials for each user and define who has access to which
             resources
           + do not need to share credentials
           + can create IAM users with read-only access to resources and distribute those
             credentials to users
    * tasks that require to be signed in as the root user
        - change account settings
        - restore IAM user permissions
        - activate IAM access to the Billing and Cost Management console
        - view certain tax invoices
        - close AWS account
        - change or cancel AWS support plan
        - register as seller in Reserved Instance Marketplace
        - configure MFA delete for S3 bucket
        - edit or delete S3 bucket policy
        - sign up for GovCloud

Artifact
--------
    * access to security and compliance reports and select online agreements
    * show compliance enabled services and documentations
    * AWS will not replicate data across Regions if certain Regions do not allow to get data
      outside of that Region
    * **Artifact Agreements**
        - can review, accept and manage agreements for an account or all accounts in OUs
        - e.g HIPAA
    * **Artifact Reports**
        - provide compliance reports from third-party auditors

Cognito
-------
    * support multiple login providers
    * unique users/devices and helps implement security best practices

GuardDuty
---------
    * analyze continuous streams of metadata generated from the account and network activity
      found in CloudTrail events, VPC Flow Logs and DNS Logs
    * uses integrated threat intelligence
    * runs independently from other AWS services
    * will not affect performance of existing workloads

Inspector
---------
    * run automated security assessment against infrastructure
    * e.g exposure of EC2 instances and vulnerabilities
    * network configuration reachability piece
    * Amazon agent: can installed on EC2 instances
    * security assessment service

KMS
---
    * Key Management Service
    * create and manage encryption keys that is used to encrypt database tables
    * can choose specific levels of access control for keys

WAF & Shield
------------
    * protects applications against DDoS attacks
    * extensive machine learning capabilities
    * **Shield Standard**
        - auto protects all customers at no cost
        - protects resources from most common, frequently occurring types of DDoS attacks
    * **Shield Advanced**
        - paid service that provides detailed attack diagnostics
        - integrates with services such as CloudFront, Route 53, ELB
    * WAF uses web application firewall to filter incoming traffic using web ACLs

`back to top <#aws>`_

Storage
=======

* `Instance stores`_, `EBS`_, `S3`_, `EFS`_
* block storage
    * when a file is updated, only the piece that changed is updated instead of overwriting
      whole series of blocks
    * e.g hard drive on computer
    * for uploading video files that get edited frequently
    * useful for complex read, write, change functions

Instance stores
---------------
    * local storage of EC2, temporary block-level storage
    * physically attached to EC2 host
    * all data will be deleted if the instance is stopped or terminated
    * when restarting the instance, the underlying host might change where the previous volume
      does not exist
    * only useful for temp data or data that can be easily recreated

EBS
---
    * Elastic Block Store
    * EBS volumes: virtual hard drives that can be attached to EC2 instances
    * separate drives from local instance store volumes and not tied directly to the host
    * data written will persist between stops and starts of EC2 instance
    * define configurations (volume and type) and provision it
    * size up to 16 TiB
    * SSD by default
    * store data in single AZ
    * AZ-level resource, EC2 and EBS need to be in same AZ
    * **EBS snapshots**
        - incremental backups of data
        - only blocks of data that have changed since the most recent snapshot are saved

* object storage
    * each object consists of data, metadata and a key
    * entire object is updated when a file in object storage is modified
    * for documents, images, video files that get uploaded and consumed as entire objects
    * useful for complete objects or only occasional changes

S3
--
    * Simple Storage Service, store and retrieve unlimited amount of data
    * data is stored as objects in buckets instead of file directory
    * maximum individual object size of 5TB
    * objects can be versioned, unversioned by default
    * every object has a URL
    * serverless, no EC2 instance required
    * create multiple buckets and store across different classes
    * useful for static website hosting
    * when choosing S3 storage class, consider how often data will be retrieved and how
      available the data need to be
    * **Standard**
        - for frequently accessed data
        - 11 nines % of durability
        - data is stored in at least three AZs
        - higher cost than other classes
        - e.g websites, content distribution, data analytics
    * **Standard-Infrequent Access (S3 Standard-IA)**
        - for data that is accessed less frequently but requires rapid access when needed
        - good to store backups  or objects that require long-term storage
        - lower storage and higher retrieval prices
        - data is stored in at least three AZs
    * **One Zone-Infrequent Access (S3 One Zone-IA)**
        - data is stored in at single AZ
        - lower storage price than Standard-IA
        - to save costs on storage and to reproduce data when failure
    * **Intelligent-Tiering**
        - for data with unkown or changing access patterns
        - small monthly monitoring and automation fee per object
        - object not accessed for 30 consecutive days is moved into Standard-IA
        - if object from Standard-IA is accessed, it is automatically moved into Standard
    * **Glacier**
        - for data that needs to be retained several years for auditing and don't need rapid
          retrieval
        - able to retrieve objects within few minutes to hours
        - simply move data or create vaults and populate them with *archives*
        - can create policy (WORM, write once/read many) and lock the vault
        - once locked, the policy cannot be changed
        - three options of retrieval and can upload data directly or use S3 Lifecycle policies
        - Lifecycle policies: can move data automatically between S3 tiers
    * **Glacier Deep Archive**
        - lowest-cost storage class for archiving
        - able to retrieve objects within 12 hours
        - consider data retrieval time when choosing Glacier and Glacier Deep Archive

* file storage
    * for use cases in which a large number of services and resources need to access the same
      data at the same time

EFS
---
    * Elastic File System
    * AWS handles the scaling and replication of files
    * multiple instances can access the data in EFS at the same time
    * only for Linux, as it uses Network File System (NFS) volumes
    * store data in and across multiple AZs
    * Regional resource, any EC2 instance in the region can write to the EFS
    * on-premises servers can access EFS using AWS Direct Connect

`back to top <#aws>`_

Exam Guides
===========

* `Cloud Practitioner`_

Cloud Practitioner
------------------
    * [official exam guide](https://d1.awsstatic.com/training-and-certification/docs-cloud-practitioner/AWS-Certified-Cloud-Practitioner_Exam-Guide.pdf)
    * includes four domains
    * **Cloud Concepts (26%)**
    * **Security and Compliance (25%)**
    * **Technology (33%)**
    * **Billing and Pricing (16%)**
    * always read the full question, try to predict answer before looking at choices, eliminate
      incorrect choices

`back to top <#aws>`_

References & External Resources
===============================

* Amazon Web Services. [2024]. AWS Glossary. Available at:
  https://docs.aws.amazon.com/general/latest/gr/glos-chap.html
* Amazon Web Services. [2024]. Overview of Amazon Web Services - AWS Whitepaper.
  Available at: https://d0.awsstatic.com/whitepapers/aws-overview.pdf
* Amazon Web Services. [2024]. AWS Cloud Essentials: GETTING STARTED GUIDE. Available at:
  https://aws.amazon.com/getting-started/cloud-essentials/
* Amazon Web Services. [2024]. AWS Blog. Available at: https://aws.amazon.com/blogs/
* Amazon Web Services. [2024]. AWS Well-Architected. Available at:
  https://aws.amazon.com/architecture/well-architected/
* Amazon Web Services. [2024]. Discover, deploy, and manage software that runs on AWS.
  Available at https://aws.amazon.com/marketplace
* Amazon Web Services. [2024]. AWS Whitepapers & Guides. Available at:
  https://aws.amazon.com/whitepapers/
* Amazon Web Services. [2024]. AWS Workshops. Available at: https://workshops.aws/
* Orban, Stephen. [2016]. A Process for Mass Migrations to the Cloud. Available at:
  https://aws.amazon.com/blogs/enterprise-strategy/214-2/
* Amazon Web Services. [2024]. Discover How AWS Leverages Contract Vehicles within the Public
  Sector. Available at: https://aws.amazon.com/how-to-buy/
* Leonhard, Michael. [2024]. cloudping.info. Available at: https://cloudping.info/
* NIST. [2012]. Cloud Computing Synopsis and Recommendations. Available at:
  https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-146.pdf
* CISPE. Buying Cloud Services in Public Sector. Available at:
  https://cispe.cloud/website_cispe/wp-content/uploads/2019/05/Public-Policy-strategy-on-Procurement-Handbook-Final-190528.pdf
* Amazon Web Services. [2024]. AWS Self Assessments. Available at:
  https://cloudreadiness.amazonaws.com/#/
* AWS Public Sector Blog Team. [2017]. Ten considerations for a cloud procurement. Available
  at: https://aws.amazon.com/blogs/publicsector/ten-considerations-for-a-cloud-procurement/

`back to top <#aws>`_
