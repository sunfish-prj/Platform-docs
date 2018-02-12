####################################
Federated Runtime Monitoring (FRM)
####################################

FRM (Federated Runtime Monitoring) consists of two components: i) Proxy and ii) Chaincode component. The development and deployment models for each of these components are discussed below.


Proxy
===========

The proxy component represents the proxies that need to be attached to the DS components to enable the interception of access requests and responses.

Development model
--------------------

The proxy component has been developed as a servlet filter in order to be compatible with the DS servlets. It is released ad a .jar dependency in such a way to be totally integrate in the Tomcat Server. The proxy consists of two Java source files, named ProxyFilter.java and CachedServletRequest.java under the sunfish.frm.proxy package.

The supplied pom.xml file contains the maven depency code snippet for the required libraries.

The supplied web.xml file, located under the src/main/webapp/WEB-INF directory, contains the servlet mapping for the servlet filter.

[A config.json file, located under the src/main/webapp/WEB-INF directory, contains configuration directives.]

There are several additional libraries supplied in the src/main/webapp/WEB-INF/lib directory. which need to be properly added into the java path during the deployment.

Installation guide
-------------------
The following steps are required to deploy and/or integrate the proxy with each DS component.

N.B. The following instructions refer to the dockerised setup of the SUNFISH Data Security Enforcement Infrastructure. Follow the doc for the deployment.

1. Download the last release of the Proxy Infrastructure tenant from the `Releases` tab in the GitHub repository (https://github.com/sunfish-prj/Federation-Monitoring/releases).

2. Once loaded the preconfigured docker containers *demotenant.tar* and *infra.tar* open the infrastructure folder and copy from the Federation-Monitoring/install/ds-infrastructure/ folder the following elements:
	* ``./params.json``	#This contains the configuration params for the ProxyFilter
	* ``./web.xml``			#This contains a new version of the configuration for Tomcat. It enables the filter-mapping.
	* ``./deploy.sh``		#This is the modified script to launch the instrastructure tenant. It load all required dependencies and configurations in Tomcat.

3. Copy ``ProxyFilter-1.0-release.jar`` download at 1. in ``Federation-Monitoring/install/ds-infrastructure/dependencies/``

4. Copy the dependencies/ inside the infrastructure folder.

Once finished you should have inside the DS module infrastructure and the tenant folders. Your infrastructure folder should looks like this:

.. code-block:: bash

		ubuntu@ubuntu:~/Data-Security/ds/doc/install/docker/infrastructure$ ls -la
			attributes
			config.sh
			dependencies
			deploy.sh
			infra.tar
			params.json
			web.xml

Usage guide
------------

In ``./params.json`` must be defined the configuration directives. Configure the following paameters:

* ServerIP #It is the IP of the localhost where the dockers are running.
* Path 		 #It is the path for calling the API of the monitoring service.

Once completed the configuration, run the ``./deploy.sh`` script to execute the infrastructure tenant with the ProxyFilter.

By using the demo application the ProxyFilter intercept requests as the following:

 .. code-block:: bash

		<?xml version="1.0" encoding="UTF-8"?>
		<Request  xmlns="urn:oasis:names:tc:xacml:3.0:core:schema:wd-17" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:oasis:names:tc:xacml:3.0:core:schema:wd-17 http://docs.oasis-open.org/xacml/3.0/xacml-core-v3-schema-wd-17.xsd" ReturnPolicyIdList="false" CombinedDecision="false">
 				<Attributes Category="urn:sunfish:attribute-category:service">
						<Attribute AttributeId="urn:sunfish:attribute:id" IncludeInResult="false">
								<AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">demo</AttributeValue>
						</Attribute>
						<Attribute AttributeId="urn:sunfish:attribute:service:zone" IncludeInResult="false">
								<AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">demozone</AttributeValue>
						</Attribute>
				</Attributes>
				<Attributes Category="urn:sunfish:attribute-category:application">
						<Attribute AttributeId="urn:sunfish:attribute:id" IncludeInResult="false">
								<AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">TBD?!!</AttributeValue>
		 				</Attribute>
		 				<Attribute AttributeId="urn:sunfish:attribute:application:zone" IncludeInResult="false">
				 				<AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">demozone</AttributeValue>
		 				</Attribute>
		 				<Attribute AttributeId="urn:sunfish:attribute:application:host" IncludeInResult="false">
				 				<AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">TBD?!!</AttributeValue>
		 				</Attribute>
						</Attributes>
				<Attributes Category="urn:sunfish:attribute-category:request">
		 				<Attribute AttributeId="urn:sunfish:attribute:request:method" IncludeInResult="false">
				 				<AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">GET</AttributeValue>
		 				</Attribute>
		 				<Attribute AttributeId="urn:sunfish:attribute:request:path" IncludeInResult="false">
				 				<AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">/demo-app/demo/ds/index.html</AttributeValue>
		 				</Attribute>
		 				<Attribute AttributeId="urn:sunfish:attribute:request:port" IncludeInResult="false">
				 				<AttributeValue DataType="http://www.w3.org/2001/XMLSchema#integer">80</AttributeValue>
		 				</Attribute>
		 				<Attribute AttributeId="urn:sunfish:attribute:request:protocol" IncludeInResult="false">
				 				<AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">http://</AttributeValue>
		 				</Attribute>
		 				<Attribute AttributeId="urn:sunfish:attribute:request:content-type" IncludeInResult="false">
				 				<AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">application/json</AttributeValue>
		 				</Attribute>
		 				<Attribute AttributeId="urn:sunfish:attribute:request:body-data" IncludeInResult="false">
				 				<AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">sfbd20812981</AttributeValue>
		 				</Attribute>
		 				<Attribute AttributeId="urn:sunfish:attribute:request:content-type" IncludeInResult="false">
				 				<AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">text/xml</AttributeValue>
		 				</Attribute>
		 				<Attribute AttributeId="urn:sunfish:attribute:request:header-parameter" IncludeInResult="false">
				 				<AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">sfhp021</AttributeValue>
		 				</Attribute>
		 				<Attribute AttributeId="urn:sunfish:attribute:request:header-parameter" IncludeInResult="false">
				 				<AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">sfhp101</AttributeValue>
		 				</Attribute>
				</Attributes>
		</Request>

and send to the Service Ledger Monitoring the following json:

 .. code-block:: bash
 
 		{
			"timeStamp":"2017-12-13 17:47:21",
			"requestorID":"TODO",
			"data":"W0RvY3VtZW50OiAgTm8gRE9DVFlQRSBkZWNsYXJhdGlvbiwgUm9vdCBpcyBbRWxlbWVudDogPFJlcXVlc3QgW05hbWVzcGFjZTogdXJuOm9hc2lzOm5hbWVzOnRjOnhhY21sOjMuMDpjb3JlOnNjaGVtYTp3ZC0xN10vPl1d",
			"dataType":"REQUEST",
			"loggerID":"PDP",
			"token":"TODO",
			"monitoringID":"/demo-app/demo/ds/index.html"
		}

Chaincode
============

The chaincode components exposes the endpoints for the PVE (Policy Violation Engine ) and the agent. The PVE is an integrated component of the FRM used to analyse the access logs. The agent endpoint is used by the FSA to forward alerts to the RI.


Development model
------------------

The current iteration of the chaincode component of FRM leverages the hyperledger fabric blockchain and the chaincode represents the smart-contract which are executed on the fabric blockchain.

This component has been developed using node.js. The flow control in the registry interface is as follows:

.. code-block:: console

	SUNFISH Component ====> frm.js --> *API.js --> hyperledger/hyperledger*.js ====> fabric ====> SUNFISH Component


The frm.js is the entry point of the chaincode component. There are different hyperledger*.js files inside the *hyperledger* directory; each of which is responsible for interacting with a particular smart-contract. There are also different *API.js files which are responsible for forwarding each request to the appropriate hyperledger*.js file. Currently, these API.js files are configured to hyperledger. However, if needed, this configuration can be changed in the config.ini file and also by developing required .js files which interact with the other blockchain.

A SUNFISH component submits a request following the SUNFISH RI specification. Based on the request path, the request is forwarded internally to the appropriate *API.js file. Then this file  forwards the request to the corresponding hyperledger*.js file where the request is handled.

Deployment guide
------------------

Follow steps are required to deploy any chaincode.

1. Prepare the hosting machine by following the instructions at: http://hyperledger-fabric.readthedocs.io/en/latest/prereqs.html

2. Setup your GOPATH environment variable as required.

3. Clone the Registry repository using the following command:

.. code-block:: bash

	https://github.com/sunfish-prj/Federation-Monitoring.git

4. cd into *Federation-Monitoring/chainComponent* directory.

3) Configure the IP address of the docker container and the id of the PVE in the config.ini file.

4) In a terminal, within the *Federation-Monitoring/chainComponent* directory, the following command needs to be issued. This starts the node server for the FRM, listening at port 8077.

    node frm.js

5) Wait until the *server started* output is printed in the terminal. This indicates that the node server for Infrastructure RI has been successfully started.
