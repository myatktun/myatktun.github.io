=======
Jenkins
=======

1. `What is Jenkins?`_
2. `Jenkins CLI`_
3. `Plugins`_
4. `Administration`_
5. `Pipeline`_
6. `Build Agents`_
7. `Blue Ocean`_



CI (Continuous Integration)
---------------------------
    * package the code, run tests (unit/integration) and security checks
    * send it to CD if everything is success

CD (Continuous Delivery/Deployment)
-----------------------------------
    * deploy the application to a system
    * Delivery
        - consists manual intervention (such as button clicks)
    * Deployment
        - code is auto deployed to a system
        - no manual intervention
    * has to be authenticated, run test against the system to be deployed on

`back to top <#jenkins>`_

What is Jenkins?
================

* open-source CI/CD suite with multiple plugins
* free or paid enterprise SaaS
* can host on any system, VM or containers
* need Java as prerequisite

`back to top <#jenkins>`_

Jenkins CLI
===========

* built-in cli that can be connected via curl/ssh

.. code-block:: sh

   java -jar jenkins-cli.jar -s http:// -auth 'user:API_TOKEN' -webSocket COMMAND
   
   # safely restart jenkins
   java -jar jenkins-cli.jar -s http:// -auth 'user:API_TOKEN' -webSocket safe-restart


`back to top <#jenkins>`_

Plugins
=======

* enhance Jenkins functionality
* can be installed from webUI or cli
* some plugins required Jenkins to be restarted after install
* plugins allow certain commands to be used in pipeline scripts

.. code-block:: sh

   # list plugins
   java -jar jenkins-cli.jar -s http:// -auth 'user:API_TOKEN' -webSocket list-plugins
   
   # install & update plugins
   java -jar jenkins-cli.jar -s http:// -auth 'user:API_TOKEN' -webSocket install-plugin
   
   java -jar jenkins-cli.jar -s http:// -auth 'user:API_TOKEN' -webSocket disable-plugin


`back to top <#jenkins>`_

Administration
==============

* security
    * has to secure the server that Jenkins is running on
    * authenticate users in security realm
    * authorize each user with different permissions
    * use plugins for extra security features
    * by default, Jenkins run builds on the built-in node
    * any builds running on the built-in node have the same level of access to the controller
    file system as the Jenkins process
    * set the number of Executors to 0 on the built-in node
    * an agent process can ask the controller process for information available to it and even
    to have the controller run certain commands when requested by the agent
    * Agent → Controller Access Control system prevents agent processes from being able to send
    malicious commands to the Jenkins controller
    * Jenkins recommend [Build Pipeline](https://plugins.jenkins.io/build-pipeline-plugin/) plugin for proper access when running builds on
    multiple agents

Manage Users
------------
    * create, delete, modify users
    * need [Role-based Authorization Strategy](https://plugins.jenkins.io/role-strategy/) plugin for role-based management

Configure System
----------------
    * show Jenkins home, location, add GitHub server, email notification
    * change any global configurations

Configure Global Security
-------------------------
    * security realm, authorization modes, agent protocols, disable API Token
    * Matrix-based security Authorization allow more in-depth security controls
    * users should have Overall/Read permission to see UI
* backup and restore
    * make sure to backup JENKINS_HOME folder, which contains config.xml, jobs
    * can use snapshots, plugins (e.g ThinBackup) or shell script to backup
    * ThinBackup can restore based on time stamps
* monitoring
    * can use Datadog, Prometheus and Grafana, JavaMelody and other plugins
    * setting jenkins server as target in prometheus

        .. code-block:: yaml

           - job_name: 'Jenkins'
               metrics_path: /prometheus/
               static_configs:
                 - targets: ['localhost:8085']


`back to top <#jenkins>`_

Pipeline
========


Jenkinsfile
-----------
    * text file that contains definitions, tells pipeline what to do
    * can have one stage per file or multi-stage pipeline by defining stages in one single file
    * if one stage fails in multi-stage, the pipeline will stop at that stage


    pipeline {
        agent any

        stages {
            stage('Build') {
                steps {
                    echo 'Building...'
                }
            }
            stage('Test') {
                steps {
                    echo 'Testing...'
                }
            }
        }
    }


* by default, all pipelines are in ``/var/lib/jenkins/workspace/``
* example Go pipeline


    pipeline {
        agent any
        tools {
            go 'go-version'
        }

        environment {
            GO11MODULE='on'
        }

        stages {
            stage('Test') {
                steps {
                    git 'my-go-app.git'
                    sh 'go test ./...'
                }
            }

            stage('Build docker image') {
                steps {
                    script {
                        app = docker.build('username/my-go-app')
                    }
                }
            }
        }

    }


* example multi-stage Go pipeline with different nodes


    pipeline {
        agent none
        stages {
            //DEV
            stage('Build Dev') {
                agent {
                  label {
                    label 'dev'
                    customWorkspace "/opt/go-app"
                  }
                }
                steps {
                    sh 'git pull'
                }
            }
            stage('Test Dev') {
                agent {
                  label {
                    label 'dev'
                    customWorkspace "/opt/go-app"
                  }
                }
                steps {
                    sh 'go test ./...'
                }
            }
            stage('Deploy Dev') {
                agent {
                  label {
                    label 'dev'
                    customWorkspace "/opt/go-app"
                  }
                }
                steps {
                  script {
                    withEnv ( ['JENKINS_NODE_COOKIE=do_not_kill'] ) {
                      sh 'go run main.go &'
                      }
                    }
                }
            }
            //PROD
            stage('Build Prod') {
                agent {
                  label {
                    label 'prod'
                    customWorkspace "/opt/go-app"
                  }
                }
                steps {
                    sh 'git pull'
                }
            }
            stage('Test Prod') {
                agent {
                  label {
                    label 'prod'
                    customWorkspace "/opt/go-app"
                  }
                }
                steps {
                    sh 'go test ./...'
                }
            }
            stage('Deploy Prod') {
                agent {
                  label {
                    label 'prod'
                    customWorkspace "/opt/go-app"
                  }
                }
                steps {
                  script {
                    withEnv ( ['JENKINS_NODE_COOKIE=do_not_kill'] ) {
                      sh 'go run main.go &'
                      }
                    }
                }
            }
        }
    }


`back to top <#jenkins>`_

Build Agents
============

* systems that run entire CI/CD pipeline workload
* can have agents anywhere, on separate machine or as container, but must be able to run java
* e.g Windows, Linux, macOS, Docker
* running builds on same Jenkins server is not recommended
* should have a dedicated server to run build agent
* using Ubuntu server as build agent
    * install Jenkins as the main Jenkins server
    * add new user: ``sudo adduser newuser``
    * add new user to sudo group: ``sudo usermod -aG sudo newuser``
    * add new credentials for 'newuser' in Jenkins server
    * create new node in Jenkins server
    * restrict the pipeline or project to only run in the build agent
* using Docker container as build agent
    * set docker image in ``agent``


    pipeline {
        agent {
            docker { image 'golang:latest'}
        }
    }


`back to top <#jenkins>`_

Blue Ocean
==========

* just a new UI with easier to use, sophisticated visualizations
* intuitive pipeline status, pipeline editor, personalization
* pinpoint precision and native integration for branch and pull requests
* not installed by default, can be installed as plugin

`back to top <#jenkins>`_
