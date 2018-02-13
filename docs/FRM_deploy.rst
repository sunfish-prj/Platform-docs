####################################
Federated Runtime Monitoring (FRM)
####################################

The installation of the FRM component is here listed. 

Proxy
===========

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


Installation Guide
------------------

