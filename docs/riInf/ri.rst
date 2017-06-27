###############################################################
Instructions for Registry Interface Deployment and Development
###############################################################

The Registry Interface is a key component of the SUNFISH architecture. It has two purposes. On one hand, it interacts with the fabric blockchain. On the other hand, it provides web interfaces for other SUNFISH components to enable the interactions between those components and the blockchain. 

It has two components **RI** and **RI Infrastructure** which are presented below. 

RI
====

RI is responsible for handling requests from other SUNFISH components in all tenants, except in an infrastructure tenant. Next, two instruction sets are presented. One describes how to deploy deploy this component. The other presents the development paradigm to aid other developers working in future.

Deployment Guide
------------------

RI can be deployed by following the steps presented below. It has been integrated with the fabric blockchain by utilising the docker container provided by the fabric project. However, thanks to its modular architecture, other blockchain can be easily integrated in future. Follow the Development Guide below to understand how it can be done.

1. Prepare the hosting machine by following the instructions at: http://hyperledger-fabric.readthedocs.io/en/latest/prereqs.html

2. Setup your GOPATH environment variable as required.

3. Clone the Registry repository using the following command:

.. code-block:: bash

	git clone https://github.com/sunfish-prj/Registry.git

4. cd into *Registry/chaincode* directory.

5. Copy the "github.com" directory from the *Registry/chaincode* directory to $GOPATH/src/

6. Clone the Registry Interface repository using the following command:

.. code-block:: bash

	git clone https://github.com/sunfish-prj/Registry-Interface.git

7. cd into *Registry-Interface/RI* directory.

8. Issue the following command to make shell scripts executable:

.. code-block:: bash

	chmod a+x channel_test.sh deployAll.sh stop.sh

9. Issue the following command to install the required node packages:

.. code-block:: bash

	npm install

10. Update the **goPath** config field in *Registry-Interface/RI/config.json* with the go path directory of the host (as printed by the :bash:`echo $GOPATH` command)

11. Update the **dockerIP** config field in *Registry-Interface/RI/config.ini* with the IP address of the docker interface (for ubuntu, use the ifconfig command to get the IP address of the docker interface for the host machine)

12. Within the *Registry-Interface/RI* directory, issue the following command in a terminal. This creates the fabric blockchain and initiates and deploys the required entities for the particular blockchain.

.. code-block:: bash

	docker-compose -f dockerCompose.yml up -d

.. role:: bash(code)
   :language: bash

13. In another terminal, use the command :bash:`docker exec -it cli bash` to connect to the cli container and then issue: :bash:`more results.txt`. Repeat :bash:`more results.txt` until the following outputs are printed. This ensures that all peers have joined the created channel.

.. code-block:: console

	SUCCESSFUL CHANNEL CREATION
	SUCCESSFUL JOIN CHANNEL on PEER0
	SUCCESSFUL JOIN CHANNEL on PEER1
	SUCCESSFUL JOIN CHANNEL on PEER2

14. In different terminals, the following commands can be used to trace the logs of the orderer and peer0 (or peer1/peer2 by changing the respective value) respectively :

.. code-block:: bash

	docker logs -f orderer

	docker logs -f peer0

15. In another terminal, within the *Registry-Interface/RI* directory, the following command needs to be issued to deploy the required smart contracts (chaincode):

.. code-block:: bash

	./deployAll.sh

16. Wait until the following output is printed. This confirms that the smart contract has been successfully deployed in the fabric blockchain. This output will be repeated all each chaincode.

.. code-block:: console

	The chaincode transaction has been successfully committed

17. In the same terminal (or in a different terminal), within the *Registry-Interface/RI* directory, the following command needs to be issued. This starts the node server for the registry interface, listening at port 8075.

.. code-block:: bash
	
	node ri.js

18. Wait until the *server started* output is printed in the terminal. This indicates that the node server for RI has been successfully started.

19. Test the interface by registering, retrieving, updating and deleting some dummy data, use the test cases from the from the testCases file. For these test cases, *docker_IP* needs to be updated accordingly. The in/index field needs to be updated accordingly for reading from the interface.

20. To get the output of the smart-contract, the following command can be issued after a single data has been registered/stored. Here, "..." represents the corresponding container name.

.. code-block:: bash

	docker logs -f peer0-peer0... 

21. Once finished, issue the following command to stop and remove the fabric containers:

.. code-block:: bash

	./stop.sh

22. Repeat the steps from step 10 to deploy the smart contracts and utilise the ri.

23. To enable the interactions between the RI and FRM/FAM, a separate instance of RI for any infrastructure tenant is required. This needs to be deployed following the instructions provided below.

Development Guide
------------------

Ri has been developed using node.js. The flow control in the registry interface is as follows:

.. code-block:: console

	SUNFISH Component ====> ri.js --> *API.js --> hyperledger*.js ====> fabric ====> SUNFISH Component

The ri.js is the entry point of the registry interface. There are different hyperledger*.js files; each of which is responsible for interacting with a particular smart-contract.
There are also different *API.js files which are responsible for forwarding each request to the appropriate hyperledger*.js file. Currently, these *API.js files are configured to
hyperledger. However, if needed, this configuration can be changed in the config.ini file and also by developing required *.js files which interact with the other blockchain.

A SUNFISH component submits a request following the SUNFISH RI specification. Based on the request path, the request is forwarded
internally to the appropriate *API.js file. Then this file  forwards the request to the corresponding hyperledger*.js file where the request is handled.

RI Infrastructure
==================

RI Infrastructure is responsible for handling requests from other SUNFISH components in an infrastructure tenant. Next, two instruction sets are presented. One describes how to deploy deploy this component. The other presents the development paradigm to aid other developers working in future.

Deployment Guide
------------------

1) If not already cloned, clone the Registry Interface project using the following command:

.. code-block:: bash

	git clone https://github.com/sunfish-prj/Registry-Interface.git

2) cd into *Registry-Interface/INF_RI* directory.

3) Configure the IP address of the hosting machine by changing the frmIP parameter in the config.ini file.

4) In a terminal, within the *Registry-Interface/INF_RI* directory, the following command needs to be issued. This starts the node server for the registry interface for the infrastructure tenant, listening at port 8076.

    node infRI.js

5) Wait until the *server started* output is printed in the terminal. This indicates that the node server for Infrastructure RI has been successfully started.

Development Guide
------------------
This follows the same pattern described in the previous section.
