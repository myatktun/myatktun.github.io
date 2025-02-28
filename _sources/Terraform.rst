=========
Terraform
=========

1. `What is Terraform?`_
2. `Providers`_
3. `Configuration`_
4. `Input/Output`_
5. `Data Sources`_
6. `Dependencies`_
7. `Built-in Functions`_
8. `State`_
9. `Modules`_
10. `Secrets`_
11. `Terraform Cloud`_
12. `Automation`_

`back to top <#terraform>`_

What is Terraform?
==================

* IaC (Infrastructure as code) provisioning tool, developed by HashiCorp, to manage
infrastructure with configuration files instead of GUI
* produces executable documentation for describing and building infrastructure
* can manage infrastructure by defining resource configurations in human-readable declarative
files that can be versioned, reused and shared
* can be automated to scale without increasing manual workload
* can manage on multiple cloud platforms and track resources changes in deployments
* providers plugins allow Terraform to interact with cloud platforms and other services via
APIs
* can even write own custom provider
* declarative configuration language describes the desired end-state for infrastructure, not
step-by-step instructions
* define infrastructure scope for project, write the configuration, initialize to install the
plugins, plan to preview changes that will be made and apply the changes to deploy
infrastructure
* Terraform keeps track of real infrastructure in a state file and uses it to determine changes
* can share state with teammates using Terraform Cloud or connect it to VCSs to manage through
version control

`back to top <#terraform>`_

Providers
=========

* define individual units of infrastructure as resources
* can put different providers' resources into reusable configurations called modules
* most providers have been written by HashiCorp and the Terraform community and can be found in
[Terraform Registry](https://registry.terraform.io/browse/providers)
* providers auto calculate dependencies between resources to create or destroy them in correct
order
* providers manage resources by communicating between Terraform and target APIs
* multiple users running the same Terraform configuration should all use same versions of
required providers
* can manage versions by specifying constraints in ``terraform {}`` block or using dependency
lock file

AWS
---


    terraform {
      required_providers {
        aws = {
          source  = "hashicorp/aws"
          version = "~> 4.16"
        }
      }
      required_version = ">= 1.2.0"
    }

    provider "aws" {
      region = "example-region"
    }



Docker
------


    terraform {
      required_providers {
        docker = {
          source  = "kreuzwerker/docker"
          version = "2.23.1"
        }
      }
    }

    provider "docker" {
      host = "unix:///var/run/docker.sock"
    }



Additional Resources
--------------------
    * [Official Providers Documents](https://developer.hashicorp.com/terraform/language/providers)

`back to top <#terraform>`_

Configuration
=============

* example configuration file to launch AWS EC2 instance


        /* main.tf */
        resource "aws_instance" "app_server" {
          ami           = "ami-07651f0c4c315a529"
          instance_type = "t2.micro"

          tags = {
            Name = "ExampleAppServerInstance"
          }
        }


    * ``terraform {}``
        - contains Terraform settings such as required providers that will be used
        - ``source`` of each provider defines optional hostname, namespace and provider type
        - providers are installed from Terraform Registry by default,
        ``registry.terraform.io/hashicorp/aws``
        - ``version`` attribute is optional, but recommended to use to have provider version
        that works well with the configuration, most recent version is downloaded if not
        specified
    * ``provider "PROVIDER_NAME" {}``
        - configure the specified provider
        - can use multiple provider blocks to manage resources from different providers
    * ``resource "RESOURCE_TYPE" "RESOURCE_NAME" {}``
        - define components of infrastructure, can be physical, virtual or logical resource
        - prefix of the ``TYPE`` maps to the name of the provider
        - ``RESORUCE_TYPE`` and ``RESOURCE_NAME`` form a unique ID for the resource, e.g
        ``aws_instance.app_server``
        - the block contains arguments to configure the resource, such as machine size, disk
        image names or VPC IDs
* ``terraform init``
    * to initialize and install required providers in ``.terraform`` folder
    * ``.terraform.lock.hcl`` file is created to specify exact provider versions used
    * ``terraform init -upgrade`` to upgrade a provider
* ``terraform fmt``
    * format the configuration file for readability
* ``terraform validate``
    * validate the configuration file syntax
* ``terraform plan``
    * view actions Terraform will carry out to provision the resource
* ``terraform apply``
    * output the changes and apply the configuration if approved
    * ``(known after apply)`` values in output means that value will not be known until the
    resource is created, such as AWS ARN for EC2 instance
    * use ``-auto-approve`` not to prompt
    * ``-/+`` means resource will be destroyed and recreated
    * ``~`` means resource will be updated in-place
* ``terraform destroy``
    * terminate resources specified in Terraform state

Migrate existing infrastructure
-------------------------------
    * can migrate existing infrastructure to Terraform management
    * configuration file for imported resource will not be generated
    * need to write configuration that matches the imported infrastructure
    * importing infrastructure could leave existing Terraform projects in an invalid state
    * make sure to backup ``terraform.tfstate`` and ``.terraform/`` before using import
    * example importing local docker container

        .. code-block:: sh

           terraform import docker_container.nginx CONTAINER_ID
           #or
           terraform import docker_container.nginx $(docker inspect --format="{{.ID}}" CONTAINER)


    * check provider documentation when importing for error handling and plan step
    * know if it is safe to apply the configuration to the resource
    * update configuration using current state and modify as necessary

        .. code-block:: sh

           terraform state show docker_container.nginx > main.tf


    * resources defined by single unique ID or tag can be imported without using ``import``
    command
        - such as docker images (``docker image inspect sha256:ID -f {{.RepoTags}}``)
        - create ``docker_image`` resource first and update the ``image`` attribute in
        ``docker_container`` resource to avoid recreating
    * import only knows the current state as reported by the provider
    * importing can be error prone due to manual steps and doesn't generate relationships
    between infrastructure
    * import doesn't detect default attributes and not all providers support it
    * imported infrastructure can be dependent on other unmanaged configuration
    * may need to set environment variables
    * import command always runs locally, does not have access to the remote backend

Troubleshoot
------------
    * **Language**
        - HCL syntax error in configuration file
        - will print out line numbers and description of error
        - check with ``terraform fmt`` and ``terraform validate``
        - ``validate`` command stops once it catches an error
    * **State**
        - state out of sync, may destroy or change existing resources
        - after fixing configuration errors, always review the state
        - ensure state is in sync by refreshing, importing or replacing resources
    * **Core**
        - Terraform core erros can be bugs
        - open GitHub issue if necessary
    * **Provider**
        - authentication, API calls and mapping resources to services errors
        - open GitHub issue for provider development team if necessary
    * **opening GitHub issue**
        - confirm Terraform and providers version with ``terraform version``
        - enable logging by setting environment variables ``export TF_LOG_COR=TRACE``,
        ``export TF_LOG_PROVIDER=TRACE``, ``export TF_LOG_PATH=logs.txt`` and run a command such
        as ``terraform refresh`` to generate log file, unset the environment variables to
        disable logging
        - can submit issue as forum topic before opening in the repository

Additional Resources
--------------------
    * [Immutabel Infrastructure](https://www.hashicorp.com/resources/what-is-mutable-vs-immutable-infrastructure/)
    * [Terraformer](https://github.com/GoogleCloudPlatform/terraformer)
    * [Forum](https://discuss.hashicorp.com/c/terraform-core/27), [Official Repository](https://github.com/hashicorp/terraform), [Community Guidelines](https://www.hashicorp.com/community-guidelines)

`back to top <#terraform>`_

Input/Output
============


Input variables
---------------
    * variables allow to write flexible and easier to re-use configuration
    * variables do not change values during Terraform commands
    * set description and type for all variables and default value when practical
    * unassigned variables are not supported
    * values will be converted to correct type if possible, use most appropriate type in
    definitions
    * **variable types**
        - string: string value
        - bool: true or false
        - number: numeric value
        - list: values of same type (start from index 0, can use functions on it,
        slice(var.LIST_VAR, start, end_exclusive)
        - map: key/values of same type
        - null: absence or omission
        - set: unordered collection of unique values of same type
        - tuple: fixed-length values of specified types
        - object: fixed set of keys/values of specified types
    * **defining input variables**


        /* variables.tf */
        variable "instance_name" {
          description = "Value of the Name tag for the EC2 instance"
          type        = string
          default     = "ExampleAppServerInstance"
        }


    * **using input variables**


        /* main.tf */
        resource "aws_instance" "app_server" {
          ami           = "ami-07651f0c4c315a529"
          instance_type = "t2.micro"

          tags = {
            Name = var.instance_name
          }
        }


    * override variable from command-line, variable value will not be saved

        .. code-block:: sh

           terraform apply -var "instance_name=newName"


    * assign values with ``terraform.tfvars`` or ``*.auto.tfvars`` without setting default


        /* variables.tf */
        variable "instance_name" {
          description = "Value of the Name tag for the EC2 instance"
          type        = string
        }

        /* terraform.tfvars */
        instance_name     = "ExampleAppServerInstance"


    * **validate variables**


        variable "instance_name" {
          description = "Value of the Name tag for the EC2 instance"
          type        = string
          default = "1234"

          validation {
            condition = length(var.instance_name) <= 4
            error_message = "EC2 instance name length cannot be more than 4 characters"
          }
        }



Output values
-------------
    * must apply configuration before using the output values as output values are stored in
    the state file


    /* outputs.tf */
    output "instance_id" {
      description = "ID of EC2 instance"
      value       = aws_instance.app_server.id
    }

    output "instance_public_ip" {
      description = "Public IP address of EC2 instance"
      value       = aws_instance.app_server.public_ip
    }


    * setting outputs to sensitive will avoid printing them out to the console
    * use sensitive outputs to share data from configuration with other modules or tools
    * ``terraform output``
        - query the output values, will not show sensitive values by default
        - ``terraform output OUTPUT_1``
        - ``-json`` for JSON-formatted output, sensitive outputs will be shown
    * outputs can be used to connect Terraform projects with other parts of infrastructure or
    other Terraform projects

`back to top <#terraform>`_

Data Sources
============

* to fetch data from APIs or other Terraform state backends
* allow to reference values from other configurations
* data sources values can be used like any other Terraform values
* list AWS availability zones of current region


    data "aws_availability_zones" "available" {
      state = "available"
      filter {
        name   = "zone-type"
        values = ["availability-zone"]
      }
    }

    output "azs" {
      value = data.aws_availability_zones.available.names
    }


* get correct AWS AMI ID for current region


    data "aws_ami" "server_ami" {
      most_recent = true
      owners      = ["amazon"]

      filter {
        name   = "name"
        values = ["ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-*"]
      }
    }


* using remote state data source
    * can only load root-level output values
    * cannot directly access resources or modules, need to add necessary outputs


    data "terraform_remote_state" "local_state" {
      backend = "local"

      config = {
        path = "./terraform.tfstate"
      }
    }

    output "local_state" {
      value = data.terraform_remote_state.local_state.outputs
    }


`back to top <#terraform>`_

Dependencies
============

* Terraform auto infers dependencies between resources in same configuration
* resources are created or destroyed in dependency order
* order declared in configuration files has no effect on the create or destroy order
* dependencies between different parts of infrastructure must be created explicitly


    resource "aws_s3_bucket" "bucket" {}

    resource "aws_instance" "instance" {
        depends_on = [aws_s3_bucket.bucket]
    }


`back to top <#terraform>`_

Built-in Functions
==================

* Terraform configuration language has built-in functions

``templatefile``
--------------
    * create resource from user data script


    # user_data.tftpl
    #!/bin/bash

    export USER=${name}

    /* variables.tf */
    variable "user_name" {
        default = "terraform"
    }

    /* main.tf */
    resource "aws_instance" "instance" {
        user_data = templatefile("user_data.tftpl", {name = var.user_name})
    }



``lookup``
--------
    * reference values from a map


    /* variables.tf */
    variable "aws_amis" {
        type = map
        default = {
            "us-east-1" = "ami-0739f8cdb239fe9ae"
            "us-west-2" = "ami-008b09448b998a562"
            "us-east-2" = "ami-0ebc8f6f580a04647"
        }
    }

    /* main.tf */
    resource "aws_instance" "instance" {
        ami = lookup(var.aws_amis, "us-east-2", "default-if-key-not-exist")
    }



``file``
------
    * read contents of a file
    * should only use with files that do not need modification


    /* mian.tf */
    resource "aws_key_pair" "ssh_key" {
        key_name = "ssh_key"
        public_key = file("~/.ssh/ssh_key.pub")
    }


`back to top <#terraform>`_

State
=====

* the state file tracks and maps resources created
* do not manually modify the state file
* example ``terraform.tfstate``

    .. code-block:: json

       {
           "resources": [
               {
                   "mode": "type of resource created (e.g managed, data)",
                   "type": "resource type (e.g aws_ami, aws_instance)",
                   "instances": [
                       "data of the resource": "e.g schema_version, attributes, dependencies"
                   ]
               }
           ]
       }


* ``terraform show``
    * inspect current state
* ``terraform state list``
    * list resources in project's state
* resources can be replaced from CLI
    * useful when system malfunction
    * ``terraform plan -replace="aws_instance.example"``
    * ``terraform apply -replace="aws_instance.example"``
    * using ``-replace`` is recommended for managing resources without manually editing the state
    file
* ``terraform state mv``
    * move resources from one state file to another or rename resources
    * resources will be updated in state file but not in configuration file
    * useful when combining modules or resources from other states without recreating the
    infrastructure
    * resource names must be unique in the destination state file

    .. code-block:: sh

       terraform state mv -state-out=../terraform.tfstate \
       aws_instance.example_new aws_instance.example_new


    * Terraform will try to destroy since the configuration file is not the same as the state
* ``terraform state rm``
    * remove resources from the state file
    * ``terraform state rm aws_instance.example_new``
    * removing the state will not delete the output values
* ``terraform import``
    * bring the resource into the state file
    * ``terraform import aws_instance.example_new $(terraform output -raw instance_name)``
* ``terraform refresh``
    * update the state file when physical resources change outside of Terraform workflow
    * does not update the configuration file
    * refresh is auto performed during ``plan``, ``apply`` and ``destroy``
    * not recommended to use the command
* ``terraform plan -refresh-only``
    * out of sync or drift state will cause unintentional destroy or recreate
    * use to inspect changes without modifying the infrastructure to match the configuration
    * recommended over ``refresh`` command
    * use the ``-refresh-only`` and ``import`` commands to reconcile the state file

`back to top <#terraform>`_

Modules
=======


benefits of modules
-------------------
    * make configuration easier to understand and organized
    * make reusable and encapsulated configuration by separating into distinct logical
    components
    * provide consistency in configurations and make easier for other teams to use
    * updating a module will also be applied to all those that use it
* a module is a set of Terraform configuration files in a single directory, even if it is only
one file

root module
-----------

remote module
-------------
Terraform Cloud or Enterprise private module registries
* real-world configurations should always use modules

best practices
--------------
    * name the provider ``terraform-<PROVIDER>-<NAME>``
    * always start writing configuration with modules in mind
    * use local modules to organize and encapsulate the code
    * use public Terraform Registry to find useful modules
    * publish and share modules with the team
* example AWS VPC module


    module "vpc" {
        source  = "terraform-aws-modules/vpc/aws"
        version = "3.18.1"
    }


    * ``source`` is required when using a module
        - given value will be searched in the Terraform Registry
        - can also be URL or local module
    * ``version`` is optional
        - recommended to include so others will also use same version
        - latest version will be loaded if not provided
    * using variables is similar to any Terraform configuration


Additional Resources
--------------------
    * [Publishing Modules to Terraform Cloud](https://developer.hashicorp.com/terraform/cloud-docs/registry/publish-modules), [Public Modules Registry](https://registry.terraform.io/browse/modules)
    * [Modules Documentation](https://developer.hashicorp.com/terraform/language/modules)

[#####](#####) [back to top](#terraform)

Secrets
=======

* can store long-live credentials, such as AWS credentials, in HashiCorp's Vault
* use Terraform's Valut provider to generate appropriately scope and shot-lived credentials to
be used by Terraform
* operators (Vault Admin) can avoid managing static, long-lived secrets with varying scope
* developers (Terraform Operator) can provision resources without direct access to the secrets
* generated token is only valid for its default ``TTL`` and in large multi-resource
configurations, the apply process may exceed the `TTL` and fail

Additional Resources
--------------------
    * [Vault](https://www.vaultproject.io/)

[#####](#####) [back to top](#terraform)

Terraform Cloud
===============

* allow to run Terraform in a remote environment with shared access to state
* teams can collaborate on infrastructure changes and securely store variables
* provide safe and stable environment for long-running Terraform processes

example setup
-------------
    * ``cloud {}`` can only be used in version 1.1.0 or higher
    * ``organization`` needs to be setup in the account first
    * ``workspaces`` will be created if necessary, existing workspaces must not have any states
    * cloud workspaces differ form CLI/normal workspaces, contain everything needed and
    function like separate working directories


    /* main.tf */
    terraform {
      cloud {
        organization = "organization-name"
        workspaces {
            name = "my-workspace"
        }
      }

      required_providers {
        aws = {
          source  = "hashicorp/aws"
          version = "~> 4.16"
        }
      }
      required_version = ">= 1.2.0"
    }


    * ``terraform login``: log into Terraform Cloud account
    * to migrate the state file to Cloud, must reinitialize the configuration with
    ``terraform init``
    * after migration, local ``terraform.tfstate`` file can be removed

Workspaces
----------
    * resources are oragnized by workspaces and operations occur in a workspace
    * CLI/normal workspaces allow multiple state files to exist within single directory
    * can use one configuration for multiple environments
    * can share data between workspaces
    * workspaces must be destroyed in order of dependency
    * can lock workspaces to prevent runs

Workflows
---------
    * CLI-driven
        - initiate operations in the terminal and run Terraform remotely or locally
        - using local execution will execute Terraform on local machine and remotely store the
        state file in the Cloud
        - ``terraform init`` will create workspace defined in ``cloud{}`` if needed
        - remote execution can be confirmed from UI or API
    * VCS-driven
        - changes pushed trigger runs in the workspace
        - must configure VCS access, create workspace and associate it with a repository
        - Speculative plans: non-destructive, plan-only runs that show the changes if a PR is
        created and runs will not show in Cloud logs, can only access from direct link attached
        to a PR
        - speculative plans can only be applied by merging to main branch
    * API-driven
        - create tools to interact with [Terraform Cloud API](https://developer.hashicorp.com/terraform/cloud-docs/api-docs)
* remote state storage keeps state and secret information off the local disk
* remote state is loaded only in memory when used

Variables
---------
    * can define workspace-specific or reusable sets of input, for configuration, and
    environment variables, usually for provider credentials
    * variable sets help avoid redefining same variables across workspaces
    * variables passed with ``-var`` will override workspace-specific ones, but will not be saved

Destroy
-------
    * **Queue destroy plan**
        - destroy all infrastructure managed by the workspace
    * **Delete from Terraform Cloud**
        - delete the workspace without destroying the infrastructure

Sentinel
--------
    * Policy-as-Code product, by HashiCorp, to enable granular policy control of infrastructure
    * language and policy framework that restricts Terraform actions to allowed behaviors
    * can be extended to use information from external sources
    * policy authors manage Sentinel policies with policy sets, named grouping of policies and
    enforcement levels
    * available in Team & Governance and Enterprise tier
    * organization owners can applyl policy sets to entire organization or specific workspaces
    * enables to treat governance requirements as applications
    * need a VCS repository to host policy configuration ([example](https://github.com/hashicorp/learn-terraform-enforce-policies))
    * Sentinel CLI validates and tests rules to develop policies
    * to run a policy, it needs data to test the policy against
    * **enforcement levels**
        - hard-mandatory: policy needs to be passed, run is halted if fails
        - soft-mandatory: similar to hard-mandatory, but allow admin to override policy
        failures
        - advisory: never interrupt the run, only show policy failures as information
    * file must follow the naming convention of ``<policy-name>.sentinel``
    * policy set names within an organization must be unique
    * policy pass will return a value of ``true`` in Terraform versions 1.1.0 and above
* in Team & Governance and Enterprise tier, Cloud estimates hourly and monthly costs for
resources and policies can be applied for cost ([example](https://github.com/hashicorp/learn-terraform-cost-estimation))

Additional Resources
--------------------
    * [Sentinel](https://docs.hashicorp.com/sentinel/)

`back to top <#terraform>`_

Automation
==========


workflow
--------
    * initialize the Terraform working directory
        - ``terraform init -input=false``
        - ``-input=false`` makes Terraform not to prompt for input as all necessary values will
        provided
    * make a plan to match current configuration
        - save the plan to local file
        - ``terraform plan -out-tfplan -input=false``
        - use additional flags such as ``-var`` or ``-var-file``
    * let operator review the plan
    * apply the changes
        - ``terraform apply -input=false tfplan``
* recommended to use a backend for remote state
* set environment variable ``TF_IN_AUTOMATION`` to any non-empty value to make output adjustments
* running ``plan`` and ``apply`` in different machines
    * after ``plan`` completes, archive entire working directory, including ``.terraform`` and save
    or archive it to be accessible, usually as build artifact
    * to run ``apply``, obtain the archive and extract at the same absolute path
    * ``plan`` and ``apply`` must be applied on same OS and CPU architecture
    * plugins must not be upgraded between two stages
    * Terraform will not detect if credentials used are different
    * plan file should be protected if it contains sensitive data
    * use environment variables for provider credentials as they are not included in the plan
    * allow and apply only one plan at a time, other existing plans must be recomputed
* use automatic approval, ``-auto-approve``, only with non-critical infrastructure
* ``plan`` can be used for pull requests
    * verify configuration without affecting real infrastructure and use hooks to trigger
    orchestration tools
    * passing sensitive data to Terraform via variables or environment variables let those who
    submit PR to see the values

multi-environment deployment
----------------------------
    * use ``terraform workspace`` to safely switch between multiple states for same config in
    single backend
    * recommended to use single backend configuration for all environments
    * setting ``TF_WORKSPACE`` environment variable will override any selection made from
    ``terraform workspace select`` command (recommended only for non-interactive usage)
    * ``terraform init -backend-config`` to use different credentials or endpoints
* using only fixed set of plugins
    * running ``init`` in automation can be overhead as plugins are downloaded every time
    * let the admin control which plugins are available in the environment
    * create a directory where Terraform will run and place the plugin executable files
    * plugin release archives can be download on [releases.hashicorp.com](https://releases.hashicorp.com/), version number on
    filenames is important
    * use ``terraform init -plugin-dir=/custom-plugins`` or place in ``terraform.d/plugins/OS_ARCH``
    * can use ``-get-plugins=false`` to prevent downloading additional plugins

using Terraform Cloud
---------------------
    * direct integration with VCS, pland an apply lifecycle orchestration, remote state storage
    * environments for operations, RBAC, UI for reviewing and approving plans
    * can use remote executions instead of running in the pipeline
    * make sure no one change infrastructure outside of the build pipeline to avoid drift and
    approve runs promptly not to let configuration stale
    * variables used in ``cloud{}`` block: ``TF_CLOUD_ORGANIZATION``, ``TF_CLOUD_HOSTNAME``,
    ``TF_WORKSPACE``, ``TF_TOKEN_<hostname>``

`back to top <#terraform>`_
