##############
Configurator 
##############

The codebase of the Configurator is available here `Configurator <https://github.com/sunfish-prj/Configurator>`_

The overall structure of the codebase is organised in the following files and directories:
	* /client/\*
	* /configurator/\*
	* /saltstack/\*
	* /Vagrantfile

******************
Installation Steps
******************
There are the following steps for creating a cluster of Kubernetes Nodes:

	1. Instantiate the Virtual Machines (configure and execute Vagrantfile)
	2. Configure the saltstack master and minions
	3. Apply kubernetes states
	4. Proceed with cluster operations

In this document we describe the setup of a Kubernetes cluster composed by two nodes.

===================
Vagrant config file
===================
The initial point for the setup process is the file *Vagrantfile*. This file contains all the information needed for the configuration of the Virtual Machines (VMs).

The minimum requirement for setting this infrastructure up is the initialization of four VMs: *Saltstack master*, *kubernetes master* and two minions.
Vagrantfile provides the configuration details for each of these four virtual machines, referred respectively as *saltmaster*, *master*, *node1*, and *node2*.

Apart of the mandatory specifications that a common configuration of a VM machine requires (like operating system, hostname, ip), in this infrastructure the VMs should have some additional configurations. 

Specifically, for the configuration of saltmaster machine are required:

Source code that saltmaster should run.
---------------------------------------
This includes two folders: salt and pillar
	1. Path to a custom salt master config file
	2. Path to a custom salt minion config file
	3. Path to master key
	4. Path to minion key

Since in Saltstack’ s perspective the other three VMs are minions, the requirement for their initial configuration are identical:
	1. Path to a custom minion config file
	2. Path to minion key

Note that to provision of the guest machines is used the Vagrant Salt provisioner, which allows the usage of the Salt states.
Also, in the configuration of the saltmaster machine, the folders clients and configurator refer to the necessary libraries and source code for the Configurator API. Configurator API is included inside the saltmaster machine, but practically it can be located to a new VM.

To boot the environment and have all the virtual machines running:
	``vagrant up``

To access the shell of a running virtual machine:
	``vagrant ssh [name]``

At the end of this process should be running the saltstack master machine and three minions. At this point, the minions are identical to each other, and they are all connected to saltmaster machine. This can be verified by executing these shell commands:

Enter saltmaster machine:
	``vagrant ssh saltmaster``

Check the status of the connected minions:
	``salt-run manage.status``

To require the deployment of the kubernete master in the minion *master*:
	``salt-run state.orchestrate k8s_orchestrate``

At this stage, the Kubernetes states may be applied to the remaining minions in order to obtain Kubernetes nodes.

=================================
Configuration of Saltstack master
=================================

	* /root/conf/\* contains configurations files of saltstack
	* /srv/salt/_states/\* contains custom state module for saltstack
	* /srv/salt/_modules/\* contains custom execution module for saltstack
	* /srv/salt/top.sls provides mapping between states files and target minions
	* /srv/pillar/top.sls provides mapping between pillar data and target minions

The Salt Master maintains States and Pillars, located respectively at /srv/salt/\* and /srv/pillar/\*.

Salt States (/srv/salt/\*)
---------------------------

Salt is a directory hierarchy that contains state files. Each state file describes one or more states to be configured and enforced on the targeted machines. 
top.sls is the first file that will be processed, and it determines the SLS files to run on particular minions.
Pillar Data (/srv/pillar/\*).

Pillar is managed in a similar way as the Salt states. Pillar data is used to configure the specific data that should be distributed to minions. 
This directory contains the top.sls file and the pillar files. Top.sls provides a mapping between the minions and the pillar files.

All the pillar files provide the data targeted to a minion, described as *key:value*.

Keys
----
/root/k8s_keys/\* contains the list of keys for the target minions.

Applying the states
-------------------

From SaltStack Master can be applied the states for the minions machines. 
The execution of state.orchestrate on saltstack master provides a stateful management of the entire infrastructure by allowing control over the application of asynchronous states on different minions.

``vagrant ssh saltmaster``

``sudo salt-run state.orchestrate k8s_orchestrate``

===========================
Configuration of Kubernetes
=========================== 
	* /srv/salt/k8s/\* contains the kubernetes formulas
	* /srv/salt/k8s_yaml/\* contains the files for kubernetes deployments
	* /srv/salt/k8s_orchestrate.sls is the file that manages kubernetes installation
	* /srv/salt/k8s_orchestrate.sls provides the configurations required to deal with the creation of the cluster. Specifically, this file defines the states that should be applied for:

Certificate Authority (CA)
--------------------------
In /srv/pillar/k8s_common.sls should be defined the list of nodes authorized to get a signed certificate by CA and the location of the public keys.

Kubernetes Master
--------------------------
In /srv/pillar/k8s_master.sls should be defined the kubernetes nodes that are going to be deployed, the labels applied to them, and the location of the yaml files for service deployment.

Kubernetes Nodes
--------------------------
and /srv/pillar/k8s_node.sls contains the configurations that should be assigned to target minions like the IP range of the cluster services.

Apply node to label
--------------------------
This action requires to define the name of the kubernetes node and the label in the pillar file /srv/pillar/k8s_master.sls.

After this, should be applied the custom state: salt "master" state.apply k8s.master.node_label 

Deploy a service
--------------------------
The file with the configuration of the service should be located at /srv/salt/k8s_yaml/* (e.g new.yaml).
This file should be included in the configurations of /srv/pillar/k8s_master.sls.
The new state should be applied: salt "master" state.apply k8s.master.deploy_yaml

Custom state module
-------------------
/srv/salt/_state/k8s_custom.py contains the implementation of functions for different custom states of kubernetes:

	* label_node_present
	* node_cordoned
	* node_uncordoned
	* node_drained
	* node_absent
	* yaml_applied
	* node_labels

Custom execution module functions
---------------------------------
/srv/salt/_modules/k8s_custom.py contains the implementation of functions for different custom modules:

	* ``get_node_list`` lists the information of all nodes in the cluster
	* ``get_pods_list`` lists the information of all pods in the cluster
	* ``get_svc_list`` lists the information of all services in the cluster
	* ``get_node`` lists the information of a single node

Configurator
---------------------------------
/root/clients/* saltstack client 

	* /srv/salt/salt_rest_api.sls provides the configuration of the saltstack client (such as keys, users, modules etc.)
	* /root/configurator/* configurator API 
	* /srv/salt/configurator.sls provides the required packages for Configurator API

To set the configurator up, is required the installation of the saltstack client library on the saltstack master machine. This library is located at /root/clients/\* (specified in Vagrantfile).
In the same way, the Configurator API is located at /root/configurator/\*. Note that, for simplicity, in Vagrantfile, is specified to install the Configurator API in SaltStack master machine, but this API can be installed in any deployed virtual machine.

************
Installation
************

	* ``sh /root/configurator/setup.sh``

	* ``sh /root/configurator/start.sh``

In the end of these steps, the minions of the saltstack, which are not configured as saltstack master or kubernetes master, will be stored as Virtual Machines resources.

Configurator API will be running at port 8443, and it can be accessed at
``https:/IP:8443/api/configurator/v1``

=========================== 
API Calls
=========================== 

	* /confVMS
		Modify virtual machine with vmID="node1"
		``curl -ik https://localhost:8443/api/configurator/v1/confVMS/node1 -H "Content-Type: application/json" -X PUT -d '{"vmID":"node1","vmType":"containerized","confID":"1"}'``.

	* /confNodes
		Configure a kubernetes node for the VM with id="node1"
		``curl -ik https://localhost:8443/api/configurator/v1/confNodes -H "Content-Type: application/json" -X POST -d '{"vmID":"node1","nodeID":"node1","labels":{"label_key":"123", "label_key2": "qwerty"}}'``.

		Remove the node with nodeID="node1" from kubernetes cluster
		``curl -ik https://localhost:8443/api/configurator/v1/confNodes/node1 -H "Content-Type: application/json" -X DELETE``

	* /jobs
		List all the async operations added in a queue
		``curl -ik https://localhost:8443/api/configurator/v1/jobs -H "Content-Type: application/json" -X GET``


