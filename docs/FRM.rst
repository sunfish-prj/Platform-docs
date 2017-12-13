#########
FRM
#########

FRM (Federated Runtime Monitoring) consists of two directories having two components: i) Proxy and ii) Chaincode component. The development and deployment models for each of these components are discussed below.


Proxy
===========

The proxy component represents the proxies that need to be attached to the DS components to enable the interception of access requests and responses.

Development model
--------------------

The proxy component has been developed as a servlet filter in order to be compatible with the DS servlets. It consists of two Java source files, named ProxyFilter.java and CachedServletRequest.java under the sunfish.frm.proxy package. 

The supplied pom.xml file contains the maven depency code snippet for the required libraries.

The supplied web.xml file, located under the src/main/webapp/WEB-INF directory, contains the servlet mapping for the servlet filter. 

A config.json file, located under the src/main/webapp/WEB-INF directory, contains configuration directives.

There are several additional libraries supplied in the src/main/webapp/WEB-INF/lib directory. which need to be properly added into the java path during the deployment.

Installation guide
-------------------
The following steps are required to deploy and/or integrate the proxy with each DS component.

1. Setup the configuration directives:
	1.1 hostingID: denotes the DS component with which the proxy is being integrated. Currently, it takes the value of either **PDP** or **PEP**. Other values can be attached, however, the ProxyFilter.java 		source needs to be updated accordingly.
	1.2 loggerID: the identifier of the hosting entity.

2. Add the dependency code snippet from the pom.xml file of the proxy to the pom.xml file of the corresponding DS component.

3. Add the code servlet mapping code snippet from the web.xml of the proxy to the web.xml file of the  corresponding DS component.

4. Add the additional libraries from the src/main/webapp/WEB-INF/lib directory to the java path during the deployment.

**TODO**: For the integration, it will also require to update the *doFilter* method of *ProxyFilter.java* file so that it can capture the supplied parameters according to the APIs of the DS component and pass it to the corresponding API of the RI. 


<<ADD REFERENCE TO THE REP GITHUB>>

Usage guide
------------

<<Example of sensed requests and resposnses>>



Chaincode
============

The chaincode components exposes the endpoints for the PVE (Policy Violation Engine ) and the agent. The PVE is an integrated component of the FRM used to analyse the access logs. The agent endpoint is used by the FSA to forward alerts to the RI.


Development model
------------------

The current iteration of the chaincode component of FRM leverages the hyperledger fabric blockchain and the chaincode represents the smart-contract which are executed on the fabric blockchain.

This component has been developed using node.js. The flow control in the registry interface is as follows:

.. code-block:: console

	SUNFISH Component ====> frm.js --> *API.js --> hyperledger/hyperledger*.js ====> fabric ====> SUNFISH Component


The frm.js is the entry point of the chaincode component. There are different hyperledger*.js files inside the *hyperledger* directory; each of which is responsible for interacting with a particular smart-contract. There are also different *API.js files which are responsible for forwarding each request to the appropriate hyperledger*.js file. Currently, these *API.js files are configured to hyperledger. However, if needed, this configuration can be changed in the config.ini file and also by developing required *.js files which interact with the other blockchain.

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