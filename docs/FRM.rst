####################################
Federated Runtime Monitoring (FRM)
####################################

FRM (Federated Runtime Monitoring) consists of the following components: 

* Proxy: it monitors the interactions among the DS components, providing the access log upon which the monitoring service is built;
* Chaincode: it is the blockchain smart contract checking that no access decision has been subverted due to hijacking of intermediate DS components (aka PEGs and PIPs see :ref:`ds-label`); 
* Policy Violation Engine: it is the component based on the formal language `FACPL <http://facpl.sf.net>`_ checking that the PDP (see :ref:`ds-label`) has not been compromised and hence access control policy circumvented. 

The development and deployment models for each of these components are discussed below.

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

Policy Violation Engine
========================


TBC
