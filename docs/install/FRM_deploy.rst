####################################
Federated Runtime Monitoring (FRM)
####################################

The installation of the FRM component is here listed. 

Proxy
===========

The proxy component has been developed as a servlet filter in order to be compatible with the DS servlets. It is released ad a .jar dependency in such a way to be totally integrate in the Tomcat Server. The proxy consists of two Java source files, named ProxyFilter.java and *CachedServletRequest.java* under the *sunfish.frm.proxy package*.

The supplied pom.xml file contains the maven depency code snippet for the required libraries.

The supplied web.xml file, located under the src/main/webapp/WEB-INF directory, contains the servlet mapping for the servlet filter.

A config.json file, located under the src/main/webapp/WEB-INF directory, contains configuration directives.

There additional libraries supplied in the `src/main/webapp/WEB-INF/lib` directory need to be properly added into the java path during the deployment.

Installation guide
-------------------
The following steps are required to deploy and/or integrate the proxy with each DS component.

N.B. The following instructions refer to the dockerised setup of the SUNFISH Data Security Enforcement Infrastructure. Follow the doc for the deployment.

1. Download the last release of the Proxy Infrastructure tenant from the `Releases` tab in the GitHub repository `release <https://github.com/sunfish-prj/Federation-Monitoring/releases>`_.

2. Once loaded the preconfigured docker containers *demotenant.tar* and *infra.tar* open the infrastructure folder and copy from the Federation-Monitoring/install/ds-infrastructure/ folder the following elements:

	* ``./params.json`` this contains the configuration params for the ProxyFilter
	* ``./web.xml`` this contains a new version of the configuration for Tomcat. It enables the filter-mapping.
	* ``./deploy.sh`` this is the modified script to launch the instrastructure tenant. It load all required dependencies and configurations in Tomcat.

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

The code to be deployed is available `here <https://github.com/sunfish-prj/Service-Ledger/tree/master/server/hyperledger-fabric/chaincode/monitoring>`_.


Installation Guide
------------------


The chaincode has been implemented for the blockchain system Hyperledger Fabric v1.0.0. Its installation and deployment instruction can be found in the guide. 

1. To install the chaincode named *monitoring* is as follow 
	 
.. code-block:: bash 

	 peer chaincode install -n ex02 -v 1.0 -p github.com/hyperledger/fabric/sunfish/chaincode/monitoring.go
	 
We proceed now with the instantiation for the code for its actual running. For the sake of simplicity, we assume a blockchain network formed by two peers; the procedure can be extended for any number of peers. 
	 
2. Instantiate the chaincode on peer0 or peer2:

.. code-block:: bash 	

		peer chaincode instantiate -o orderer0:7050 --tls $CORE_PEER_TLS_ENABLED --cafile $ORDERER_CA -C mychannel -n ex02 -v 1.0 -c '{"Args":["init","a","100","b","200"]}' -P "OR ('Org0MSP.member','Org1MSP.member')" 
	
As a result, a new docker container is now created to manage the chaincode, the following log is showed::
		
		peer2       | 2017-06-20 18:32:40.189 UTC [dockercontroller] Start -> DEBU 3b6 Start container dev-peer2-ex02-1.0
		peer2       | 2017-06-20 18:32:40.189 UTC [dockercontroller] getDockerHostConfig -> DEBU 3b7 docker container hostconfig NetworkMode: e2ecli_default
		peer2       | 2017-06-20 18:32:40.190 UTC [dockercontroller] createContainer -> DEBU 3b8 Create container: dev-peer2-ex02-1.0
		peer2       | 2017-06-20 18:32:40.192 UTC [dockercontroller] Start -> DEBU 3b9 start-could not find image ...attempt to recreate image no such image
		peer2       | 2017-06-20 18:32:40.192 UTC [chaincode-platform] generateDockerfile -> DEBU 3ba
		peer2       | FROM hyperledger/fabric-baseos:x86_64-0.3.0
		peer2       | ADD binpackage.tar /usr/local/bin
		peer2       | LABEL org.hyperledger.fabric.chaincode.id.name="monitoring" \
		peer2       |       org.hyperledger.fabric.chaincode.id.version="1.0" \
		peer2       |       org.hyperledger.fabric.chaincode.type="GOLANG" \
		peer2       |       org.hyperledger.fabric.version="1.0.0-snapshot-ecc29dd" \
		peer2       |       org.hyperledger.fabric.base.version="0.3.0"
		peer2       | ENV CORE_CHAINCODE_BUILDLEVEL=1.0.0-snapshot-ecc29dd
		peer2       | ENV CORE_PEER_TLS_ROOTCERT_FILE=/etc/hyperledger/fabric/peer.crt
		peer2       | COPY peer.crt /etc/hyperledger/fabric/peer.crt
		peer2       | 2017-06-20 18:32:51.945 UTC [dockercontroller] deployImage -> DEBU 3bb Created image: dev-peer2-ex02-1.0
		peer2       | 2017-06-20 18:32:51.945 UTC [dockercontroller] Start -> DEBU 3bc start-recreated image successfully
		peer2       | 2017-06-20 18:32:51.945 UTC [dockercontroller] createContainer -> DEBU 3bd Create container: dev-peer2-ex02-1.0


.. NOTE:: the ``-o`` flag indicates the orderer ``address:port`` who handle the channel; the ``--tls`` takes a boolean and indicates whether we are using TLS or not (it's true in our environment, look at the docker-compose file); the ``--cafile`` indicates the file with the certificate of the Orderer (ignore it for now); the ``-c`` flag indicates the args to give to the chaincode in a json format and finally ``-P`` is referred to the endorsement policy (we will see later in this course).
	
	
Now a new docker container should be up with the chaincode execution on the peer. Via the command  ``docker ps`` the new docker can be shown. While via the command ``docker attach <ID>`` where ID is the CONTAINER ID discovered in the previous step with ``docker ps``, the docker can be accessed to issue execution. 
	
3. To query the chaincode on the initialised argument to check if they have been correctly stored

.. code-block:: bash
	
		peer chaincode query -C mychannel -n ex02 -c '{"Args":["query","a"]}'

while to run an execution the following command can be executed

.. code-block:: bash

		peer chaincode invoke -o orderer0:7050  --tls $CORE_PEER_TLS_ENABLED --cafile $ORDERER_CA -C mychannel -n ex02 -c '{"Args":["invoke","a","b","10"]}'
		

