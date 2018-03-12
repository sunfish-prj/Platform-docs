##########################################
Federated Administration Monitoring (FAM)
##########################################

Dependencies 
============

Install the dependencies 

- OS:

	* Windows 7 and newer
	* Windows Server 2008 R2 and newer

- `DotNetCore 1.1.4 Windows Hosting <http://download.microsoft.com/download/6/F/B/6FB4F9D2-699B-4A40-A674-B7FF41E0E4D2/DotNetCore.1.0.7_1.1.4-WindowsHosting.exe>`_
- `Internet Information Services Manager <https://docs.microsoft.com/en-us/aspnet/core/publishing/iis?tabs=aspnetcore1x>`_

To check that all the decencies have been set up, execute

.. code-block:: bash

 	dotnet --version
  
	-> Microsoft .NET Core Shared Framework Host

		Version  : 1.1.0
		Build    : 928f77c4bc3f49d892459992fb6e1d5542cb5e86


Administration Manager set-up
====================================

To set the service, execute the following commands

.. code-block:: bash

  git clone https://github.com/sunfish-prj/Administration-Manager.git 
  cd Administration-Manager/manager
  dotnet SUNFISH.dll


The server is now running and listening on the port chosen in the *hosting.json*. file (e.g. 80).  

The Administration Manager is expected to interact with the:
- `Service Level Agreements Manager <https://github.com/sunfish-prj/SLA-Manager>`_
- `Service Ledger Interface <https://github.com/sunfish-prj/Service-Ledger-Interface.git>`_

The *urls* and *ports* for the above applications may be altered in the *appsettings.production.json* config file.

For the Administration Manager to make use of the mock up IDM, set the *UseIDM* variable in the *appsettings.production.json* to true.

- The current IDM url is *49.118.99.72* using port *44001* 
- For the default username & password contact Alexander Tanti.


SLA Manager set-up
=========================

To set the service, execute the following commands

.. code-block:: bash

  git clone https://github.com/sunfish-prj/SLA-Manager.git 
  cd SLA-Manager
  dotnet Sunfish.SLAMService.dll


The server is now running and listening on the port chosen in the *hosting.json*. file (e.g. 80).  

The SLA Manager is expected to interact with the:

- `Service Ledger Interface <https://github.com/sunfish-prj/Service-Ledger-Interface.git>`_

The *urls* and *ports* for the above application may be altered in the *appsettings.production.json* config file.


