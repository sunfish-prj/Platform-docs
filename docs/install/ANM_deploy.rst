###################
Anonymisation (ANM)
###################

This is the installation for the anonymisation component. 

The anonymization service has a single dependency, namely Java8 run time environment. 

The service installation procedure is very simple:

1.	Place AnonymizationService.jar in your target directory.
2.	Place AnonymizationService_lib/ directory in the same target directory.
3.	Create an 'application.properties' file and set the port number. 

For example, the application.properties can contain the following line:
`server.port=50001`

To run the service, execute the following command:

.. code-block:: bash

	>  java -jar AnonymizationService.jar

The service is now ready to accept REST calls.

Anonymisation Interface (ANI)
==============================

**Install the dependencies**

Nodejs v6.x
Npm v3.x
Releases and installation guides can be found on the official websites here and here

To check that all the dependencies have been set up, execute
.. code-block:: bash

 $ node -v 
 -> v6.1.0
 $ npm -v
 -> 3.10.6

**Anonymisation Interface set-up**

To set the service, execute the following commands

.. code-block:: bash

 $ git clone https://github.com/sunfish-prj/Anonymisation-Interface.git
 $ npm start
 
The server is now running and listening on the port chosen in the config/server_config.yaml file (e.g. 50001).

The anonymisaiton interface is expected to interact with the anonymisation component in the Service Ledger Interface, whose url and port are defined in the configuration file config/default.yaml.
