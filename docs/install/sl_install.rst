.. _sl-inf-label:

####################
Service Ledger (SL)
####################


Dependency
===========

Install the dependencies 

- *Node.js v6.x*
- *Npm v3.x*

Releases and installation guides can be found on the official web-sites `node <https://nodejs.org>`_ and `npm <https://www.npmjs.com/>`_.

To check that all the decencies have been set up, execute

.. code-block:: bash 

  $ node -v
  -> v6.1.0
  $ npm -v
  -> 3.10.6

Note that you probably also need to have installed on the machine *make* and *gyp*; if not installed, execute also

.. code-block:: bash 

  $ apt-get install build-essential git
  $ npm install -g node-gyp 

Remember to execute the installation as superuser.

Additionally, to install the underlying platform we have two different options

1. *MongoDB* to be installed according to the used OS. To check installation outcomes

.. code-block:: bash 

  $ mongo --version
  -> MongoDB shell version v3.4.6

2. *Hyperledger Fabric* blockchain to be installed according to the `guide <https://hyperledger-fabric.readthedocs.io/en/release-1.0/getting_started.html>`_.


Service Ledger Interface
=========================

To set the service, execute the following commands

.. code-block:: bash 

  $ git clone https://github.com/sunfish-prj/Service-Ledger-Interface.git
  $ cd Service-Ledger-Interface/server
  $ npm start

The server is now running and listening on the port chosen in the *config/default.yaml*. file (e.g. 8089). You can use the *client-stub interface* http://localhost:8089/docs.  

The Service-Ledger-Interface is expected to interact with the Service Ledger whose *url* and *port* are defined in the configuration file *config/default.yaml*.


Service Ledger 
===============


To set the service, execute the following commands

.. code-block:: bash 

  $ git clone https://github.com/sunfish-prj/Service-Ledger.git
  $ cd Service-Ledger/server

To set up the ServiceLedger server, edit the file *config/default.yaml* properly. In this file it can be set to use MongoDB or Hyperledger Fabric. 

In case the server is using MongoDB, you should also start it before ServiceLedger. See the corresponding command wrt your os `here <https://docs.mongodb.com/manual/administration/install-community/>`_.
Similarly, if using Hyperledger Fabric, you need a running Fabric cluster to gather all information related to its docker containers.

Now you can start the ServiceLedger server:

.. code-block:: bash

  $ npm start

The server is now running and listening on the port chosen in the *config/default.yaml*. file (e.g. 8090). You can use the [client-stub interface](http://localhost:8090/docs).  


Usage Guide
=============

The Service Ledger Interface API is the expected entry-points for the SUNFISH platform components. 

The Service Ledger API is a general-purpose invocation for blockchain underlying platform. The following parameters are expected: 

*	Chaincode: the name of the chaincode to invoke.
*	Function: the name of the function of the chaincode to invoke. 
*	Argument: the arguments to give in input to the function.

Therefore, this API has been implemented to realise the monitoring functionality of the chaincode and, most of all, for the remaining number of chaincode part of the federation; namely anonymisation, key value store and the smart contract used for the Use Case 1. 
