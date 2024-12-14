====
Helm
====

1. `What is Helm?`_
2. `Components`_
3. `Basics`_
4. `Helm Charts`_

What is Helm?
=============

* can be considered as package manager/automation tool for Kubernetes
* treat Kubernetes objects as an app, as part of a package as a group
* instead of modifying objects, only need to tell Helm what package to be modified
* knows which objects to change based on the package name
* no need to manage each Kubernetes object
* can install, upgrade, rollback or uninstall with only single command
* can specify desired values for the package at install time by only editing single file
* can be downloaded from [official Helm site](https://helm.sh/docs/intro/install/)

Helm 2 vs 3
-----------
    * extra component, Tiller, has to be installed in the cluster for Helm 2 to communicate
    Kubernetes did not have RBAC and CRD at that time
    * as Tiller is run in god mode, there are security concerns for the cluster
    * Tiller is not required anymore after RBAC and CRD
    * Helm 3 is simpler and better designed
    * **3-Way Strategic Merge Patch**
        - Helm 2 does not create new revision when objects are manually changed from kubectl,
          so rollback is not possible after manual change
        - Helm 3 compares current chart, previous chart and live state, 3-way, for rollbacks

`back to top <#helm>`_

Components
==========


Charts
------
    * collection of files that contain instructions to create Kubernetes objects
    * used to install applications into the cluster
    * different charts can be found in public repository like docker images, called
    [Helm Hub or Artifact Hub](https://artifacthub.io/)
    * ``values.yaml``: store configurable values for the chart
    * ``chart.yaml``
        - store information about the chart, such as chart API Version, app version, name
        - ``apiVersion`` field is added in Helm 3 to differentiate charts built in the past
        - additional fields in Helm 2 will be ignored and can result in unexpected behavior
        - every chart must have its own ``version``, not same as ``appVersion`` field
        - ``type: application`` is used for deploying applications
        - ``type: library`` is a chart that provides utilities that help building charts
    * **Chart Directories**
        - templates: templates directory
        - values.yaml: configurable values
        - chart.yaml: chart info
        - license: chart license
        - readme: readme file
        - charts: dependency charts

Release
-------
    * single installation of app, created when a chart is installed
    * each release can have multiple revisions, snapshots of the app
    * every change create a new revision
    * can install multiple releases based on same chart

Metadata
--------
    * data such as installed release, charts used and revision state
    * saved in the cluster as secrets

`back to top <#helm>`_

Basics
======

* ``helm search hub SEARCH_WORD``: search chart in the hub or repositories
* ``helm repo add REPO_NAME REPO_URL``: add repository before installation
* ``helm install NAME CHART_NAME``: install chart to cluster
* ``helm list``: list all releases
* ``helm uninstall NAME``: uninstall and delete all objects
* ``heml repo SUB_COMMAND``: interact with repositories

Chart Customization
-------------------
    * charts are installed with default values
    * can change values from command line with ``--set`` flags
        - ``helm install --set PARAM_1="VALUE_1" --set PARAM_2="VALUE_2" NAME CHART_NAME``
    * create new ``custom-values.yaml`` and use it to override default values
        - ``helm install --values custom-values.yaml NAME CHART_NAME``
    * pull the chart first and edit ``values.yaml`` file
        - ``helm pull CHART_NAME`` or ``helm pull --untar CHART_NAME``
        - ``helm install NAME LOCAL_CHART_DIR``

Lifecycle Management
--------------------
    * helm keep track of releases with revision numbers
    * ``helm upgrade NAME CHART_NAME``: upgrade app, will create a new revision
    * ``helm history NAME``: list releases/lifecycle of an app with details
    * ``helm rollback NAME REVISION_NUMBER``: rollback to specific revision, create new revision
    with similar configuration as given revision
    * some charts require administrative access for upgrade
    * rollbacks do not recover data
    * chart hooks are required to backup and restore data, especially in databases

`back to top <#helm>`_

Helm Charts
===========

* versatile and can automate any kind of Kubernetes package installation
* similar to installation wizards on Operating Systems
* can write a chart to do operations any time upgrade/rollback is performed
* ``helm create NAME``: create skeleton chart directory structure
* use Go templating language for dynamic chart values, such as ``{{ .Release.Name }}-myapp``
* available objects for template directive: Release, Chart, Capabilities, Values

Verifying Charts
----------------
    * **Lint**
        - validate YAML format
        - ``helm lint LOCAL_CHART_DIR``
    * **Template**
        - validate templating
        - ``helm template LOCAL_CHART_DIR`` or ``helm template NAME LOCAL_CHART_DIR``
        - use with ``--debug`` to display error in generated YAML file
    * **Dry Run**
        - validate chart works with the cluster
        - ``helm install NAME CHART_NAME --dry-run``

Functions
---------
    * transform data from one format to another, ``{{ FUNCTION ARGUMENT }}``
    * ``{{ upper .Values.image.repository }}``: convert to upprecase
    * ``{{ quote .Values.image.repository }}``: add quotes around the text
    * ``{{ replace "x" "y" .Values.image.repository }}``: function with three arguments, replace
    'x' in the text with 'y'
    * ``{{ default "myimage" .Values.image.repository }}``: default value to be used if not
    provided in the values.yml file
    * arguments not in quote are considered as variables
    * check [official docs](https://helm.sh/docs/chart_template_guide/function_list/) for more functions

`back to top <#helm>`_
