###################
Data Masking (DM)
###################

This is the installation for the data masking component. 

The masking service has a single dependency, namely Java8 run time environment. 

The service installation procedure is very simple:

1.	Place MaskingService.jar in your target directory.
2.	Create an 'application.properties' file and set the port number.  For example, the application.properties can contain the following line: server.port=50002

3.	Create/Modify the 'sunfish.properties' file (located within the jar) and set URI's for the Service Ledger. For example:

riStoreUri=http://localhost:60005/r/put
riReadUri=http://localhost:60005/r/get
riDeleteUri=http://localhost:60005/r/delete

To run the service, execute the following command:

.. code-block:: bash

	>  java -jar MaskingService.jar

The service is now ready to accept REST calls

