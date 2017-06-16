########################################
Data Security Enforcement Infrastructure
########################################

The SUNFISH data security enforcement infrastructure is responsible for regulating and securing access to services in a federated cloud environment.

It consists of the following components:
 * The **Policy Enforcement Gateway** (PEG) responsible for enforcing decision regarding whether access to a resource is granted or not and which obligations need to be observed (if any). This component serves as the main entry point to a service protected by the SUNFISH DS enforcement infrastructure.
 * An accompanying **Proxy** enabling the non-SUNFISH-aware applications to utilise the benefits of the SUNFISH platform.
 * A **Policy Decision Point** (PDP) evaluating a decision request, indicating whether access to a service should be granted or not. In addition, Obligations such as *data masking*, for example can be part of the decision.
 * **Policy Information Points** (PIPs) delivering information to the PEG and the PDP to enhance decision requests.
 * A **Policy Administration Point** (PAP) providing an interface for administration data security policies.
 * The **Registry Interface** responsible for storing, managing and delivering policies to the PDP for evaluation. Setup an operation of the RI is described separately, since it is operated independently of the other components.
 * A **Masking Service** providing *data masking* capabilities to the PEG. Like the RI, the masking service is operated independently of the other components and therefore also discussed separately.

Typically, the enforcement infrastructure will be deployed among different *tenants*. The following setup instructions are based on a minimal example consisting of one *infrastructure tenant* and a *service tenant*.


The infrastructure tenant will typically house the PDP, any number of PIPs and the PRP.
The service tenant hosts the actual service to be protected by the enforcement infrastructure as well as the PEG, any number of PIPs and the proxy to maintain backwards compatibility to non-SUNFISH-aware clients. As outlined initially, the PEG located at the service tenant serves as the main entry point, responding to incoming requests, which can either be submitted directly to the PEG, or through the proxy.
In case the requests was directed to the proxy, responses are also interpreted by the proxy and reduced in such a way that non-SUNFISH-aware applications are able to interpret it correctly (albeit losing expressibility in the process).

In-depth descriptions on how to set up a service tenant and an infrastructure tenant are available. These include step-by-step instructions to deploy the enforcement infrastructure on existing Java application servers. In addition, a streamlined, deployment-script-based setup as well as an automated, easy-to-use, self-contained, two-step, docker-based setup is provided for jump-starting a SUNFISH deployment.

Setting-Up a Service Tenant
===========================
It is assumed that a service is already running in the service tenant.

Step-By-Step Setup
------------------
Although not recommended, the SUNFISH data security enforcement infrastructure can be deployed following the succeeding steps. However, depending on the deployment use case, additional steps or adaptions to either configuration or system components may still be necessary. For demonstration purposes a two tenant setup is assumed.
The sample configuration ships with precompiled Tomcat applications, which can be found in the respective ``webapps`` directory of either tenant. Additionally, a sample configuration for the service tenant can be found in the respective ``conf`` directory.
To deploy the service tenant follow these steps:

  * Copy the content of the provided ``./tomcat/webapps`` directory to ``CATALINA_HOME/webapps`` directory
  * Copy the content of the provided ``./tomcat/conf`` directory to ``CATALINA_HOME/conf`` directory
  * Copy the content of the provided ``./proxy/`` directory to any desired directory (referred to as ``PROXY_HOME``)


In a divergent deployment scenario, the respective configurations of the SUNFISH components and the SUNFISH proxy need to be adapted individually. To start the SUNFISH data security enforcement infrastructure simply start your local Tomcat instance and execute the ``start.sh`` script, located in your ``PROXY_HOME`` directory.

Using the Deployment Script
---------------------------
The attached deployment script is an easy way to automatically setup a service tenant. For this, the following two steps are necessary:

 * Adapt the configuration if necessary (``config.sh``)
 * Execute the deployment script (``./deploy.sh``)

The deployment script will automatically create all necessary resources and copy them to their designated destination. No further steps are necessary. To start the SUNFISH data security enforcement infrastructure simply start your local Tomcat instance and execute the ``start.sh`` script, located in your ``PROXY_HOME`` directory.


Configuration Directives
^^^^^^^^^^^^^^^^^^^^^^^^
The infrastructure tenant features several configuration options before installation. The following parameters are available:

 * ``TOMCAT_PORT``: Defines the port of the local Tomcat instance
 * ``CATALINA_HOME``: Defines the home directory of the local Tomcat instance (e.g. (``/usr/local/tomcat/``)
 * ``PEP_URL_PDP``: Defines the URL of the designated *PDP* for the *PEP*
 * ``PEP_URLS_PIPS``: Defines the possible *PIPs* available to the *PEP*. Multiple URLs can be specified, separated by a comma
 * ``PEP_ZONE``: Defines the tenant name the *PEP* is located in
 * ``PIP_DATABASE``: Defines possible database values for the *PIP*. Each setting consists of a key and a value. In general three entries are necessary in order to setup a new service inside the service tenant:

   * **Host for ID**:  Assign a hostname to a specific service. The key must be in the format ``host.<service_id>``. The value represents a single URL to the designated service.
   * **Tenant for ID**: Assign a service to a specific tenant. The key must be in the format ``zone.<service_id>``. The value defines the tenant the service is located at.
   * **PEP for Tenant**: Assign a *PEP* to a specific tenant. The key must be in the format ``pep.<tenant name>``. The value represents a single URL to the designated *PEP*.

 * ``PROXY_HOME``: Defines the home directory of the SUNFISH proxy (e.g. (``/usr/local/proxy/``)
 * ``PROXY_IP``: Defines the IP address the SUNFISH Proxy will run on
 * ``PROXY_PORT``: Defines the port the SUNISH Proxy will listen to
 * ``PROXY_PEP``: Defines the URL of the designated *PEP* for the SUNFISH Proxy


Dockerised Setup
----------------
The docker-based deployment also features a configuration file containing essentially the same (at this point mostly self-explanatory) directives and a deployment script. This script has to be invoked after editing the configuration file just as it is the case for the regular deployment-script-based setup.

To actually deploy the docker container, once the configuration file has been adapted, the following steps need to be performed:

 * The preconfigured docker container *tenant.tar* needs to be loaded: ``docker load -i tenant.tar``
 * The deployment script has to be executed (``./deploy.sh``)

This should start a docker container, inside which the proxy is running on ``PROXY_PORT`` and the PEG and the PIP are running as web applications on a Tomcat server on ``TOMCAT_PORT``. Both ports are mapped to their respective counterparts on the host machine.


Setting-Up a Service
--------------------
To add a new service to the SUNFISH data security enforcement infrastructure, the following steps are necessary:


* Add a `host` for the `service id` to the configuration file ``config.sh`` or, if the SUNFISH tenant has already been setup, to the configuration file located in ``CATALINA_HOME/conf/sunfish/pip/database/pip_database.config`` 
* Add a `tenant` for the `service id` to the configuration file ``config.sh`` or, if the SUNFISH tenant has already been setup, to the configuration file located in ``CATALINA_HOME/conf/sunfish/pip/database/pip_database.config``. It is important to note that this step needs to be performed for all operational tenants, as long as the PIP database containing the service configuration is not replicated between all tenants.
* Add a `pep` for the `tenant` of the `service` to the configuration file ``config.sh`` or, if the SUNFISH tenant has already been setup, to the configuration file located in ``CATALINA_HOME/conf/sunfish/pip/database/pip_database.config``. It is important to note that this step needs to be performed for all operational tenants, as long as the PIP database containing the service configuration is not replicated between all tenants.
* Restart your local Service Tenant Tomcat in order to apply the changes

Adding Policies
--------------------
By default, any deployed service requires a dedicated policies in order for the SUNFISH data security enforcement infrastructure to work. Policies can be added via the *PAP* and the defined **API** (see also Chapter `SUNFISH Policy Administration Point (PAP) API`). A sample policy, allowing access to a defined service is shown below:

.. code-block:: xml

    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <Policy xmlns="urn:oasis:names:tc:xacml:3.0:core:schema:wd-17" xmlns:ns2="urn:sunfish" PolicyId="urn:sunfish:policy:demo-proxy-https" Version="1.0" RuleCombiningAlgId="urn:oasis:names:tc:xacml:1.0:rule-combining-algorithm:deny-overrides">
        <Description>Demo Permit-All Policy </Description>
        <Target>
            <AnyOf>
                <AllOf>
                    <Match MatchId="urn:oasis:names:tc:xacml:1.0:function:string-equal">
                        <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">129.27.142.49</AttributeValue>
                        <AttributeDesignator Category="urn:sunfish:attribute-category:service" AttributeId="urn:sunfish:attribute:id" DataType="http://www.w3.org/2001/XMLSchema#string" MustBePresent="true"/>
                    </Match>
                    <Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-starts-with">
                        <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">/demo-app/demo/</AttributeValue>
                        <AttributeDesignator Category="urn:sunfish:attribute-category:response" AttributeId="urn:sunfish:attribute:request:path" DataType="http://www.w3.org/2001/XMLSchema#string" MustBePresent="false"/>
                    </Match>
                </AllOf>
                <AllOf>
                    <Match MatchId="urn:oasis:names:tc:xacml:1.0:function:string-equal">
                        <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">129.27.142.49</AttributeValue>
                        <AttributeDesignator Category="urn:sunfish:attribute-category:service" AttributeId="urn:sunfish:attribute:id" DataType="http://www.w3.org/2001/XMLSchema#string" MustBePresent="true"/>
                    </Match>
                    <Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-starts-with">
                        <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">/demo-app/demo/</AttributeValue>
                        <AttributeDesignator Category="urn:sunfish:attribute-category:request" AttributeId="urn:sunfish:attribute:request:path" DataType="http://www.w3.org/2001/XMLSchema#string" MustBePresent="false"/>
                    </Match>
                </AllOf>
            </AnyOf>
        </Target>
        <Rule RuleId="urn:sunfish:rule:permit" Effect="Permit">
            <Target/>
        </Rule>
    </Policy>



Setting-Up an Infrastructure Tenant
===================================

Step-By-Step Setup
------------------
Although not recommended, the SUNFISH data security enforcement infrastructure can be deployed following the succeeding steps. However, depending on the deployment use case, additional steps or adaptions to either configuration or system components may still be necessary. For demonstration purposes a two tenant setup is assumed.
The sample configuration ships with precompiled Tomcat applications, which can be found in the respective ``webapps`` directory of either tenant. Additionally, a sample configuration for the infrastructure tenant can be found in the respective ``conf`` directory.
To deploy the service tenant follow these steps:

  * Copy the content of the provided ``webapps`` directory to ``CATALINA_HOME/webapps`` directory
  * Copy the content of the provided ``conf`` directory to ``CATALINA_HOME/conf`` directory

In a divergent deployment scenario, the respective configurations of the SUNFISH components need to be adapted individually. To start the SUNFISH data security enforcement infrastructure simply start your local Tomcat instance.


Using the Deployment Script
---------------------------
The attached deployment script is an easy way to automatically setup an infrastructure tenant. For this, the following two steps are necessary:

 * Adapt the configuration if necessary (``config.sh``)
 * Execute the deployment script (``./deploy.sh``)

The deployment script will automatically create all necessary resources and copy them to their designated destination. No further steps are necessary. To start the SUNFISH data security enforcement infrastructure simply start your local Tomcat instance.


Configuration Directives
^^^^^^^^^^^^^^^^^^^^^^^^
The infrastructure tenant features several configuration options before installation. The following parameters are available:

 * ``TOMCAT_PORT``: Defines the port of the local Tomcat instance
 * ``CATALINA_HOME``: Defines the home directory of the local Tomcat instance (e.g. (``/usr/local/tomcat/``)
 * ``PAP_URL_RI``: Defines the URL of the designated **Registry Interface** for the *PAP*
 * ``PDP_URLS_PRPS``: Defines the possible *PRPs* available to the *PDP*. Multiple URLs can be specified, separated by a comma
 * ``PDP_URLS_PIPS``: Defines the possible *PIPs* available to the *PDP*. Multiple URLs can be specified, separated by a comma
 * ``PRP_URL_RI``: Defines the URL of the designated **Registry Interface** for the *PRP*
 * ``PIP_DATABASE``: Defines possible database values for the *PIP*. Each setting consists of a key and a value. In general, no additional values are necessary for the *PIP* in the infrastructure tenant.


Dockerised Setup
----------------
The docker-based deployment also features a configuration file containing essentially the same (at this point mostly self-explanatory) directives and a deployment script. This script has to be invoked after editing the configuration file just as it is the case for the regular deployment-script-based setup.

To actually deploy the docker container, once the configuration file has been adapted, the following steps need to be performed:

 * The preconfigured docker container *infrastructure.tar* needs to be loaded: ``docker load -i infrastructure.tar``
 * The deployment script has to be executed (``./deploy.sh``)

This should start a docker container, inside which the PDP, the PRP and the PIP are running as web applications on a Tomcat server on ``TOMCAT_PORT`` which is mapped to the same port on the host machine.



