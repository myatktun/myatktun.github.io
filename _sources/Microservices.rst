=============
Microservices
=============

1. `What are microservices?`_
2. `Principles`_
3. `Development`_
4. `Communication`_
5. `Code Base`_

What are Microservices?
=======================

* `Microservices Application`_, `Monolith vs Microservices`_
* factors such as easily accessible cloud infrastructure, tools and automation drive toward
microservices
* microservices enables to manage complexity at granular level
* a single microservice should be manageable by a single developer or small team
* each service should run on its own deployment schedule and be able to update independently
* each developer/team is responsible for development, testing, deployment and operations of
the service(s) they own
* individual microservice can be external for customers or only internal, with access to a
database, file storage or method of state persistence
* in a well-designed system, services must collaborate with each other to provide features and
functions of greater application

Microservices Application
-------------------------
    * a microservices application, distributed application, is a complex adaptive system
    because of its constituent parts
    * each component should be small, manageable and easy to understand
    * components are separate processes, with each on physically or virtually separate
    computer, and communicate via network
    * application can be in different forms, flexible and suitable for any situations
    * the services do different jobs, depending on business domain
* orchestration: automated management of services

Monolith vs Microservices
-------------------------
    * **Monolith**
        - a great option for creating a throw-away prototype, to validate business idea first
        - all code runs in same process with no barriers
        - can become a mess as size increases and can be difficult for new developers to
        understand
        - a code change can break the entire application and gives arise to deployment fear
        - testing can be difficult to scale as it consumes resources as entire application and
        becomes more expensive to run
    * **Microservices**
        - with the right tools, microservices prototyping can be as easy as with monolith
        - before, microservices were only used where value of solution is more than cost of
        implementation
        - as it became cheaper to replace monolithic apps, components of distributed systems
        reduce to microservices
        - each service can has dedicated resources
        - physical infrastructure can be shared between many services to be cost effective
        - can have fine-grained control, minimize deployment risk and choose own tech stack
        - error when updating a service cannot break the entire application and easier
        rollback as small part of the system rather than the whole
        - faster boot that makes system easier to scale as it can be replicated quickly
        - easier to test and troubleshoot
        - each component should be able to thrown away and be rewritten with different tech
        within weeks
        - multiple tech stacks can work together in the cluster using shared protocols and
        communication mechanism
        - microservices create structure that can be converted to newer tech stacks
        - more difficult and complex to develop, and has steep learning curve
* microservices as an architectural pattern is a tool that gives many ways to manage complexity
and force for **better design**
    * do not over design or try to future proof the architecture
    * always start with simple design
    * apply continuous refactoring during development, then a good design will emerge naturally
* duplicated code is accepted in microservices, as a trade for higher tolerance
* process boundaries make more difficult to share code

`back to top <#microservices>`_

# Principles of Microservices


single responsibility principle
-------------------------------
    * each microservice should be as small and simple as possible
    * each service should cover only single conceptual area of business
    * updating or adding a service should be easy

loose coupling
--------------
    * connections between services should be minimal
    * do not share information unless necessary
    * can help when changing the application into new configurations

high cohesion
-------------
    * all the code in a microservice belong together
    * all the code solves problem in the service's responsible area
    * a service should not solve more than one problem or has larger responsibility area
    * domain driven design works well for microservices

`back to top <#microservices>`_

Development
===========

* start with simplest code and keep it simple as possible
* simple changes are easy to understand, test and integrate
    * test each service independently as restarting a single service is faster
    * integration testing can be difficult when application grows larger
    * mocking: testing technique where dependencies are replaced with simulated alternative
* use tools, techniques, processes and patterns as the application becomes complex
    * use same versions of systems and tools in development as in production
    * install less in production environment to reduce security issues
    * deploy tools in-cluster or use third-party ones based on business requirements
* take out as much code as possible to isolate the problem when troubleshooting
* it's better for a microservice not to start than operate on wrong configuration
* use containers to abstract resources required by a microservice
    * as containers can virtualize OS and hardware, resources can be divided on one computer
    among many services
    * build container images, bootable and immutable snapshots of servers including necessary
    assets, for easier and faster deployment
    * containers names should be independent of the underlying technique
    * if possible, run commands at container startup to cache on host OS
* freedom to effect change in the future without knock-on effects is important
* control which services should be exposed and which to restrict access

stateless cluster
-----------------
    * cluster can be destroyed and rebuilt without loss of data
    * can use blue-green deployment for production rollouts
    * only need to switch DNS record to point to the new version and can be switched back to
    the old one if problems occur
* use one database per microservice if possible
    * sharing databases between services can have architectural and scalability problems
    * restrict data access only to the underlying code so that changes in structure of data
    over time can be hidden
    * avoid using a database as integration point or interface between microservices
* it's beneficial to go production while the application is still small
    * to get user feedback and adapt and build features
    * building a CD pipeline and deploying is easier

`back to top <#microservices>`_

Communication
=============

* communication is the key between microservices

direct messaging (synchronous communication)
--------------------------------------------
    * messages are sent and responded directly and immediately between services
    * used when a service should be invoked immediately to perform a task
    * also used for triggering a direct action on another microservice
    * one controller microservice can cause to perform a strict series of behaviors across
    multiple microservices, but the controller becomes single point of failure
    * recipient service can't ignore or avoid the incoming message
    * requires tight coupling between microservices
    * useful to coordinate behaviors in explicit way or well-defined order
    * can only target single service at a time
    * not easy to use when a single message should be received by multiple recipients
    * e.g messages sent with HTTP requests

indirect messaging (asynchronous communication)
-----------------------------------------------
    * has intermediary between services
    * both sender and receiver don't need to know about each other
    * looser coupling between microservices
    * the sender doesn't even know if the other service will receive the message
    * the receiver cannot send a direct reply
    * can't be used when direct response is required
    * used for important events that don't need a direct response
    * can make architecture more difficult
    * increases security, scalability, extensibility, reliability and performance
    * no single service that orchestrate others
    * allows more complex and resilient networks of behaviors
    * messages won't be lost even if the microservices fail as the messages aren't acknowledged
    when a service crashes and they will be delivered to another service to be handled
    * e.g RabbitMQ, which uses Advanced Message Queuing Protocol (AMQP)
    * **single-recipient messaging**
        - one-to-one indirect messaging between services
        - can have multiple senders and receivers
        - only single service will receive each individual message
        - useful when distributing a job that should be handled only by the first one that is
        capable of dealing with
        - will make sure that a job is done only once within application
    * **multiple-recipient messaging**
        - one-to-many broadcast style, one service sends that message but many others can
        receive
        - mostly used for notifications
* can use direct or indirect messaging to load balance handling of a message by one of a
collection of services

RabbitMQ
--------
    * assert queue
        - multiple microservices can assert a queue
        - checking and creating the queue if not exist
        - the queue is created once and shared between all participating services
    * incoming message must be parsed manually as RabbitMQ doesn't natively support JSON
    * it only sees the message as a blob of binary data
    * can use any efficient binary format for message payload
    * queue should auto deallocate when microservices disconnect to prevent memory leak

`back to top <#microservices>`_

Code Base
=========


Monorepos
---------
    * related changes in different projects can be in the same pull-request
    * **Nx**
        - ``npx create-nx-workspace``, ``yarn create nx-workspace``
        - ``nx [target] [project]``, targets (serve, build, lint, test, e2e) from ``project.json``
        - types of libraries: Feature, UI, Data-access, Utility
        - ``nx g lib feature --directory feature1 --appProject project1 --tags type:feature``,
        create library in ``libs/feature1/feature``, ``--appProject`` make the library routable
        inside specified application, ``--tags`` are add to ``project.json``
        - ``nx g component GlobalStyles --project project1-folder1 --export``, create component
        in ``libs/project1/folder1/src/lib/global-styles``, ``--project`` specifies as found in
        ``projects`` section of ``nx.json``, ``--export`` exports the component in ``index.ts`` file
        - ``nx g app --help``, ``nx g lib --help``, ``nx g component --help``
        - ``@nrwl/nx/enforce-module-boundaries`` define libraries dependencies based on tags,
        even circular dependency is not allowed, need to set ``allowCircularSelfDependency: true``
        - ``nx graph``, show dependency graph
        - ``nx affected:graph``, show affected graph (uses Git history and compare with main
        branch to determine which projects are affected, specify base with ``--base`` or
        ``deafultBase`` in ``nx.json``)
        - ``nx affected:build``, build affected apps
        - ``nx affected:test``, test affected projects
        - ``nx affected:lint``, lint affected projects
        - ``nx affected:e2e``, run e2e tests on affected projects
        - projects at the bottom of the dependency run first
        - ``--maxParallel``, specify parallel tasks (3 by default)
        - ``nx print-affected --type=app --select=projects``, list affected apps
        - ``nx print-affected --type=lib --select=projects``, list affected libraries
        - listing affected projects can be useful in CI
        - Nx uses computation caching to run tasks faster, will replay the same output if
        computation hash is the same (stored in ``node_modules/.cache/nx`` and can specify under
        ``tasksRunnerOptions`` in ``nx.json``)
        - ``nx g @nrwl/express:app api --no-interactive --frontend-project=frontend``, generate
        express app boiler plate (``--frontend-project`` add proxy configuration)
        - ``nx run-many --target=serve --projects=api,frontend``, start multiple applications
        - ``nx g @nrwl/node:lib shared-models --no-interactive``, generate utility library
        - ``nx format:check``, ``nx format:write``, checks and formats files with Prettier

`back to top <#microservices>`_
