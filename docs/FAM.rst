######################################################
Federated Administration and Monitoring (FAM)
######################################################


The Federated Administration and Monitoring (FAM) component is the management plane of the SUNFISH FaaS federation. It provides federation administrators with a graphical interface to configure, manage and monitor the services that are made available to the federation. The FAM captures the information submitted by the administrators, validates it, and dispatches that information to the respective low-level sub-components.


The FAM provides administrators with a set of functions such as to join and leave the federation, publish services and administrator access policies. It then relies on other SUNFISH components to execute the low-level instructions. The FAM has 3 sub-components being the Deployment Manager, Configurator and SLA Manager as depicted below. 


.. image:: images/FAM.pdf

The high-level interactions orchestrated by the FAM to federate a new member cloud (Cloud Federation phase, see :ref:`faas-label` ) and to make an already federated member cloud leave the federation (Federation Leaving phase, see :ref:`faas-label` ). The logic to coordinate these operations is encapsulated in the Deployment Manager component, which instructs the Configurator and consequently the IWM to execute the required actions.

Thus, the flow of interactions for the Cloud Federation phase can be summarised as follows

*	The FAM asks the Deployment Manager to federate a new member cloud.
*	The Deployment Manager instructs the IWM Adapter to establish a connection to such member cloud.
*	The IWM  interacts with this member cloud at the infrastructure level to install the software required to make it configurable for the next step.
*	The Configurator deploys required SUNFISH components over the resources provided by the new member cloud, according to a deployment plan decided by the Deployment Manager.

Similarly, for the Federation Leaving phase, the high-level sequence of interactions is as follows

*	The FAM asks the Deployment Manager to make an already federated member cloud leave the federation.
*	The Deployment Manager commands the Configurator to un-deploy all the SUNFISH components currently placed on the resources provided by the member cloud.
*	The Configurator applies such un-deployment and thus removes all the SUNFISH software currently installed on the member cloud.
*	Then the Deployment Manager asks the IWM Adapter to remove all the software that were installed during the Cloud Federation phase to establish the initial connection.


=======================================
Service Level Agreement Manager (SLAM)
=======================================







=============
Configurator
=============




===================
Deployment Manager
===================



















