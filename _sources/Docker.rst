======
Docker
======

1. `What is Docker?`_
2. `Docker Architecture`_
3. `Dockerfile`_
4. `Compose`_
5. `Network`_
6. `Docker from Scratch`_
7. `Docker Swarm`_

`back to top <#docker>`_

What is Docker?
===============

* it's not a VM
* Image
    * your starting point
    * what you produce as an output
    * great for experimenting
* Container
    * a running image
    * isolated process
    * has it's own file system, permissions
    * OS specific
* Host
    * the machine docker is installed on
* Community Edition: set of free docker products
* Enterprise Edition: certified and supported container platform with enterprise addons (e.g.
  image management, image security, universal control plane to manage and orchestrate container
  runtime)
* unlike VMs, containers are not meant to host an OS  but a specific process or task
* once a task is complete, container will exit
* container only lives as long as the process inside it is alive
* by default, container does not listen to standard input unless "-it" provided

.. code-block:: sh

   # run container from an image (with interactive, tty, auto remove when exits)
   # will download if image not found locally
   docker run -it --rm ubuntu /bin/bash
   
   # download an image, but not run
   docker pull ubuntu
   
   # list all running containers
   docker ps
   
   # list all containers
   docker ps -a
   
   # run the container, ID can be minimum as long as its unique
   docker start CONTAINER_ID
   
   # attach to the running container by ID, CTRL-p CTRL-q to detach from current
   docker attach CONTAINER_ID
   
   # detach from current container
   docker detach CONTAINER_ID
   
   # send exit signal to the running container
   docker stop CONTAINER_ID
   
   # remove the container
   docker rm CONTAINER_ID
   
   # force remove the container and the volume
   docker rm -fv CONTAINER_ID


* can make multiple containers based off of the same image
* containers are isolated
    * they don't know each other, they don't talk to each other, they don't share a file system

    .. code-block:: sh

       # run and detach (run in background)
       docker run -it -d --rm --name ubuntu1 ubuntu /bin/bash
       docker run -it -d --rm --name ubuntu2 ubuntu /bin/bash
       docker run -it -d --rm --name ubuntu3 ubuntu /bin/bash
   
       # attach to running container by name
       docker attach NAME



Storage & File System
---------------------
    * /var/lib/docker
        * storage drivers (aufs, zfs, btrfs, device mapper, overlay, overlay2)
        * containers
        * image
        * volumes
    * "docker run" creates writable Container Layer on top of Image Layer, which is read only
    * Container Layer only lives as long as the container
    * docker uses COPY-ON-WRITE mechanism
    * containers are transient, if removed, files cannot be recovered
    * Volume creates mount point between the host and the container
    * use bind mount only in development

    .. code-block:: sh

       docker run -v HOST_DIR:CONTAINER_DIR
   
       # volume mounting
       # create volume under /var/lib/docker/volumes/
       docker volume create my_volume
       # docker will auto create volume if mount without creating volume first
       docker run -v my_volume:/tmp/new_dir ubuntu /bin/bash
   
       # bind mounting
       # mount current directory
       docker run -v $(pwd):/tmp/new_dir ubuntu /bin/bash
   
       # bind mount read only, container cannot change files
       docker run -v $(pwd):/tmp/new_dir:ro ubuntu /bin/bash
   
       # to prevent overwriting the folder in container (anonymous volume)
       # removing node_modules in host will not affect the folder in container
       docker run -v $(pwd):/tmp/new_dir -v /new_dir/node_modules ubuntu /bin/bash
   
       # verbose method
       docker run --mount type=bind,source=$(pwd),target=/tmp/new_dir ubuntu /bin/basah
   
       # -w set the working directory for the process that's being executed
       # this command runs the container and app, but doesn't bound any ports to the host
       docker run -it --rm --name node -d -v $(pwd):/src -w /src node:16-alpine node app.js
   
       # can verify that app is running by logging
       docker logs NAMES -f


* PORT mapping

    .. code-block:: sh

       # Ports of NetworkSettings is empty since no port binding is made
       docker inspect CONTAINER_ID
   
       # port binding, omitting HOST_PORT will randomly choose one
       docker run -p HOST_PORT:CONTAINER_PORT
   
       # bind port to host's 8080
       docker run -it --rm --name node -v $(pwd):/src -w /src -dp 8080:3000 node:16-alpine node app.js



ENV variables
-------------
    * can find variables with inspect command, under "Config":{"Env"}

    .. code-block:: sh

       # pass env variable manually
       docker run -e VARIABLE=value CONTAINER
   
       # use env variables from .env file
       docker run --env-file ./.env


`back to top <#docker>`_

Docker Architecture
===================


Docker Daemon
-------------
    * responsible for pulling images and starting containers
    * manage volumes, networks, and DNS

REST API
--------
    * provides access to the daemon
    * available locally at /var/run/docker.sock

    .. code-block:: sh

       sudo curl --unix-socket /var/run/docker.sock http://docker/v1.41/containers/json -v | jq



Docker CLI
----------
    * client, simply making API requests

Docker Engine
-------------
    * Docker CLI: uses REST API to talk to Docker Daemon, can be on another system
    * REST API: to talk to daemon
    * Docker Daemon: manage docker objects such as images, containers, volumes, networks

    .. code-block:: sh

       # using docker cli from different system
       docker -H=remote-docker-engine-IP:2375 run nginx


    * processes are running on the same host but separated into own containers using namespace
        * nginx.service might have PID 123 on host, but PID 1 on container
    * no restriction of how much resources a container can use
        * docker uses cgroups to restrict resources

        .. code-block:: sh

           # container does not take more than 50% CPU of host
           docker run --cpus=.5 ubuntu
   
           # max memory 100MB
           docker run --memory=100m ubuntu



Docker on Windows
-----------------
    * **Docker Toolbox**
        * original support for legacy machines
        * contains Virtualbox, Docker Engine, Docker Machine, Docker Compose, Kitematic GUI
        * requires 64bit, Windows 7 or higher, Virtualization is enabeld
        * purely runs linux containers
    * **Docker Desktop for Windows**
        * use Hyper-V instead of Oracle Virtualbox
        * auto create Linux system and run docker on that system
        * only support Windows 10 Enterprise/Pro Editions or Windows Server 2016
        * default to Linux Containers
        * must set if want to use Windows Containers
    * **Windows Container Types**
        * Windows Server: works like Linux Containers, kernel is shared
        * Hyper-V Isolation: each container is run within optimized VM for kernal isolation,
          Windows 10 Pro/Enterprise Editions only support this type
    * **base images**
        * Windows Server Core
        * Nano Server: headless deployment for Windows Server, like alpine
    * Virtualbox and Hyper-V cannot coexist on the same host

Docker on Mac
-------------
    * Docker toolbox: same as Windows, macOS 10.8 or newer
    * Docker Desktop for Mac
        * uses HyperKit
        * requires macOS 10.12 or newer, hardware must be 2010 or newer
    * no Mac based images or containers

`back to top <#docker>`_

Dockerfile
==========

* an instruction set to create image and run commands
* always start with *FROM* instruction
* build in layered architecture, allows to restart from particular layer if fails
* each layer only stores the changes from previous layer
* all layers are cached, helps rebuild faster
* once a layer changes, all following layers are re-created as well

.. code-block:: Dockerfile

   # example Dockerfile
   FROM node:16-alpine
   
   # create environment variable
   ENV PORT 3000
   # EXPOSE doesn't affect building container
   EXPOSE $PORT
   
   # set the working directory, can use "RUN mkdir"
   WORKDIR /src
   
   # installing packages first is good for optimization with cache when rebuilding image
   COPY package.json .
   RUN yarn install
   
   # use copy instead of volume mounting
   # when rebuilding, usually only this step changes so building wil be faster
   COPY . /src
   
   # command instruction, anything specified on the cmd line will append to ENTRYPOINT
   ENTRYPOINT ["node"]
   
   # will execute this when container starts
   CMD ["node", "app.js"]


.. code-block:: sh

   # cannot use 'docker run'
   docker build -t TAG_NAME PATH
   
   # view build history
   docker history IMAGE_NAME
   
   # "hello.js" will be appended to ENTRYPOINT, thus "node hello.js" will execute
   # default from CMD will be used if "hello.js" is omitted
   docker run IMAGE_NAME hello.js
   
   # overwrite ENTRYPOINT command from node to nodemon
   docker run --entrypoint nodemon IMAGE_NAME


* using **multi-stage** build
    * each "FROM" instruction starts a new build stage
    * can selectively copy artifacts from one stage to another
    * all files and tools used in first stage will be discarded once completed
    * final image is created only in the last stage and will be smaller
    * only last commands are the image layers

    .. code-block:: Dockerfile

       # Build stage (1st stage)
       FROM maven AS build
       WORKDIR /app
       COPY myapp /app
       RUN mvn package
   
       # Run stage (2nd stage)
       FROM tomcat
       COPY --from=build /app/target/file.war /usr/local/tomcat/webapps
       EXPOSE 8080
       ENTRYPOINT ["java", "-jar", "/usr/local/lib/demo.jar"]



.dockerignore
-------------
    * like .gitignore file, ignore files when COPY in building image
    * example files and folders to ignore
        * node_modules, build, .dockerignore, .gitignore, .git, Dockerfile, creds, *.env, charts/*,
          \*.yml, \*.log, \*\*/coverage
        * also add Documentation, Dependencies, Tests
    * identify the files needed for build context, add everything else to .dockerignore
    * use smaller base images for containers, use multi-stage builds
    * decouple Dockerfile from env variables, reduce as much as possible
    * use separate images for development and deployment, keep images simple
    * keep it transparent and understandable, no shame in copying the good parts

* sharing on dockerhub repo
    * can test repo on [playwithdocker](https://labs.play-with-docker.com/)

    .. code-block:: sh

       # tag exisiting image to push
       docker tag IMAGE_NAME USER_NAME/REPO_NAME
       # tag when build
       docker build -t USER_NAME/REPO_NAME
   
       docker push USER_NAME/REPO_NAME



docker registry
---------------
    * default is docker hub (image: Registry/USER_ACCOUNT/IMAGE_REPO)

        .. code-block:: sh

           # image: docker.io/nginx/nginx
           docker run nginx


    * can use private registry, cloud services provide private by default

        .. code-block:: sh

           docker login private-registry.io
           docker run private-registry.io/apps/myapp


    * deploy own private registry

        .. code-block:: sh

           # image exposes its API on 5000
           docker run -d -p 5000:5000 --name registry registry:2
   
           # to push own image, first tag with private registry url
           docker image tag my-image localhost:5000/my-image
           docker push localhost:5000/my-image
   
           # can noww pull from local
           docker pull localhost:5000/my-image


* linking containers (**deprecated**)

    .. code-block:: sh

       # will create an entry in /etc/hosts files in webapp container (with hostname & internal IP)
       docker run --link CONTAINER_NAME:HOST webapp


`back to top <#docker>`_

Compose
=======

* higher abstraction tool, handles multiple containers at a time
* can be version controlled, open specification
* represents environments, not production-ready tool, just for development
* yml file to describe an environment that we want to run
* only applicable for running on single docker host
* services: containers that are going to start with inside of the dockerfile
* networks: for isolation
* version 1: cannot specify different network and startup order, no "services"
* version 2: use "services", must specify version number at the top of file, auto create
  dedicated network, no need to use "links", introduce "depends_on" feature
* version 3: similar to version 2, support docker swarm
* "depends_on" only handles boot order, docker doesn't know if the container is up and
  running everything
* can use "compose up" without stopping if docker-compose.yml file is changed

.. code-block:: yaml

   # example docker-compose file
   version: "3.9"
   
   services:
     node:
       image: node:16-alpine
       ports:
         - "5000:3000"
       networks:
         - webnet
       build:
         # specify Dockerfile to build
         context: .
         dockerfile: Dockerfile
   networks:
     webnet:


.. code-block:: sh

   # stand up particular node service inside of the compose file, omit node to stand all services
   docker compose -f ./docker-compose.yml up node
   
   # force build before starting containers
   dokcer compose up -d --build
   
   docker compose rm -f
   
   # remove anonymous volumes
   docker compose rm -v


* don't build container every changes
* mount source code into a dev container if possible

separate config for dev & prod
------------------------------
    * can separate yml files for dev and prod with one Dockerfile

        .. code-block:: Dockerfile

           FROM node:16
   
           WORKDIR /app
           COPY package.json .
   
           ARG NODE_ENV
           RUN if [ "$NODE_ENV" = "development" ]; \
                   then yarn install; \
                   else yarn install --production=true; \
                   fi
   
           COPY . .
   
           ENV PORT 3000
           EXPOSE $PORT
   
           CMD ["node", "index.js"]


    * make files docker-compose.yml, docker-compose.dev.yml, docker-compose.prod.yml

        .. code-block:: yaml

           # docker-compose.yml file
           version: "3.9"
   
           services:
             node-app:
               build: .
               ports:
                 - "3000:3000"
               environment:
                 - PORT=3000
               depends_on:
                 - mongodb
   
             mongodb:
               image: mongo
   
           # docker-compose.dev.yml file
           version: "3.9"
   
           services:
             node-app:
               build:
                 context: .
                 args:
                   NODE_ENV: development
               volumes:
                 - ./:/app:ro
                 - /app/node_modules
               environment:
                 - NODE_ENV=development
               command: yarn dev
   
           # docker-compose.prod.yml file
           version: "3.9"
   
           services:
             node-app:
               build:
                 context: .
                 args:
                   NODE_ENV: production
               environment:
                 - NODE_ENV=production
               command: node index.js


        .. code-block:: sh

           # compose up each file with -f flag
           docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d
           # rebuild for production
           docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d --build


* using multi-stage in Compose

    .. code-block:: yaml

       # example docker-compose file
       version: "3.9"
   
       services:
       node:
         build:
           context: .
           target: dev
         ports:
           - "5000:3000"
         networks:
           - webnet
       networks:
       webnet:


* configuring local dev
    * create files for each of the configuration options
    * mount the files into container

    .. code-block:: yaml

       # example docker-compose file
       version: "3.9"
   
       services:
       mysql:
         image: mysql
         volumes:
           - ./config:/config
         environment:
           - MYSQL_USER_FILE: /config/MYSQL_USER
           - MYSQL_PASSWORD_FILE: /config/MYSQL_PASSWORD
       networks:
       webnet:


* running end-to-end tests
    * create containers that bundle test tools together
    * e.g. Selenium has images with bundled Firefox/Chrome browsers
    * '--exit-code-from' flag will tell compose to watch one service, tear the stack down when it
      exits, and pass along the exit code
    *     .. code-block:: sh

       docker compose -f docker-compose-test.yml -p tests up --exit-code-from tests


`back to top <#docker>`_

Network
=======

* anything on the network can talk to others on that network but not those that aren't there
* docker auto creates 3 networks: bridge, none, host

bridge
------
    * private internal network created by docker host, only one bridge created
    * all containers attach to this by default and get internal IP of 172.17.x.x
    * containers use this internal IP if required

host
----
    * attach to host to access containers externally
    * will remove network isolation between docker host and container
    * do not require port mapping if attach to host
    * will not be able to run multiple containers on the same port

none
----
    * isolated, not attach to any network and cannot be accessed

* refer to the service name to talk to one container from another as DNS is built into
  docker (but does not work with default bridge network)

.. code-block:: sh

   docker network ls
   docker run -it --rm --network NETWORK_NAME IMAGE
   
   # create custom bridge network
   docker network create --driver bridge --subnet 182.18.0.0/16 NETWORK_NAME


* docker has built-in DNS server
* containers can reach each other using CONTAINER_NAME
* docker uses network namespaces to create separate namespace for each container
* uses virtual ethernet pairs to connect containers

`back to top <#docker>`_

Docker from Scratch
===================

* the most basic base image that you start from
* bare bones as much about running a single process
* doesn't even have shell
* only useful for executing a lightweight native process

.. code-block:: sh

   # build go app in scratch
   docker run --rm -v $(pwd)/src:/src -w /src golang:1.18.0-alpine3.15 go build -v -o app
   
   # create image
   docker build -t dfs-scratch -f Dockerfile .
   
   # run container
   docker run --rm dfs-scratch
   docker rmi dfs-scratch


`back to top <#docker>`_

Docker Swarm
============

* lacks auto scaling
* combines multiple docker machines into single cluster and helps take care of high
  availability and load balancing
* designate one of multiple hosts to be Swarm Manager or master
* key component is the Docker service: one or more instances of single app or service that
  run across the nodes in the cluster

.. code-block:: sh

   # run on Swarm Manager
   docker swarm init --advertise-addr IP
   # must be run on Manager, like "docker run"
   docker service create --replicas=3 -p 8080:8080 my-app
   
   # run on worker nodes
   docker swarm join --token <token>
   
   # list nodes within swarm
   docker node ls
   
   # list stacks
   docker stack ls
   
   # list services in a stack
   docker stack services STACK_NAME
   
   # list services from all stacks
   docker service ls


.. code-block:: yaml

   # docker-compose.prod.yml file
   version: "3.9"
   
   services:
   node-app:
     # swarm related configs
     deploy:
       replicas: 8
       restart_policy:
         condition: any
       update_config:
         parallelism: 2
         delay: 15s
     build:
       context: .
       args:
         NODE_ENV: production
     environment:
       - NODE_ENV=production
     command: node index.js


.. code-block:: sh

   # deploy using docker swarm
   docker stack deploy -c docker-compose.prod.yml STACK_NAME


`back to top <#docker>`_
