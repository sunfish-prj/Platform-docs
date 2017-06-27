###############################################################
Instructions for deploying chaincode
###############################################################

The current iteration of SUNFISH Registry leverages the hyperledger fabric blockchain and the chaincode represents the smart-contract which are executed on the fabric blockchain.

Currently, the chaincode has been written in Go lang and is hosted at: https://github.com/sunfish-prj/Registry

Within the *chaincode/github.com* directory of the repository, there are several directories. In each directory, there is a chaincode for a particular functionality of the **RI**. For example, the *chaincode/github.com/alert* directory contains a chainconde called *alert.go* which is used by corresponding RI endpoint to store and retrive an alert in the blockchain and so on. These directories also contain a file called **Dockerfile** which is used to deploy any particular chaincode in the corresponding container.

Deployment Guide
------------------

Follow steps are required to deploy any chaincode.

1. Prepare the hosting machine by following the instructions at: http://hyperledger-fabric.readthedocs.io/en/latest/prereqs.html

2. Setup your GOPATH environment variable as required.

3. Clone the Registry repository using the following command:

.. code-block:: bash

	git clone https://github.com/sunfish-prj/Registry.git

4. cd into *Registry/chaincode* directory.

5. Copy the "github.com" directory from the "Registry/chaincode" directory to $GOPATH/src/

6. Once copied, no other additional step is required. The copied chaincode will be automatically deployed in the container by the deployment script of the RI.
