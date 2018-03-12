##########################
Networking Infrastructure
##########################

This is a page for helping the setup the network and access services needed for connecting different clouds.

******************
Network Component
******************

This text details the configuration of the SUNFISH Network service, which is the component realizing the Cross-Cloud Networking functionalities (see SUNFISH deliverable ID5). The *aggregation* of networks relizes Network OSI layer visibility among different *sections* (i.e., networks), which realizes a single SUNFISH tenant. This is achieved using site-to-site VPN channels, provided by multiple instances of the Network component VMs.

===================
Planning phase
===================
Before setting-up the network services, it is necessary to: 

	P1. - Plan the networks for the Sections, and the Sections for the SUNFISH Tenants:

		1. plan the address space of the set of SUNFISH Sections, which has to form a SUNFISH Federation. This task depends on the maximum number of services to be deployed on each of the Section. As an example, for a /24 Network the maximum number of services which can be deployed (total usable hosts) in the Section is 253. 
		2. define what Sections are aggregated to form the SUNFISH Tenants.

	P2. - Plan the VPN channels topology among clouds.
		Given a certain set of SUNFISH Tenants composed by Sections, there are in general multiple VPN topologies which can be used for their implementation. The criterias to be utilized when choosing a VPN topology are outside the scope of this document. The idea however is to try to minimize the number of VPN connections while avoiding troughput bottlenecks and single points of failture. In the simplest case a hub-and-spoke topology is considered. In this case, for aggregating a set of Sections, there are the following conditions:

		1. (deployment) one instance of the Network service is deployed in each cloud, serving alle the sections belonging to the cloud
		2. (configuration) a single instance of the Network service is configured as a VPN Server, while the remaining are configured as VPN Clients connected to the Server
		3. (routing) the VPN Clients and Server are configured to realize the Tenants.

	P3. - Decide other parameters related to networking aspects:

		1. IP and ports the Network services, as seen from outside the federation. The IP and ports are those which are used by a VPN servers and clients externally to a cloud.
		2. IP and ports the Network services, as seen from inside the federation. A convention for the Network IP address defines a certain IP address in each Section to be reserved as gateway for inter-section communication (i.e., a default gateway for the federated services). As an example, the IP *.*.*.254 can be reserved as a convenction in each section for the inter-tenant communication.
		3. what are the security parameters for the VPN connections which will be used. The security parameters determines the kind of security measurement adopted for the VPN channels.

===================
Configuration phase
===================

Based on the plan phases, there are the following task to the beformed:

	T1. - Configuration at cloud level:
		1. for each cloud, it has to be created a cloud Tenant (to not be confused with a SUNFISH tenant) reserved to SUNFISH. Then the virtual network(s) associated to sections (point P1) has to be created
		2. for each cloud, the VMs dedicated to the instances of Network service (in the simplest case, just one) has to be instantiated. Given that a Network service serves a number *n* of sections, its VM has to have *n+1* NICs: {LAN_1, LAN_2, ... LAN_n, WAN}, where the WAN interface is the one directly exposed to a static cloud's public IP
		3. a proper IP address has to be assigned to the various LAN NICS, accordingly to the network convenction of point P3
		4. for each cloud, there is the need to configure a set of firewall rules at cloud level, allowing traffic from/to the cloud's tenant (specifically, from/to the IP forwarded to the WAN interface). This depends on the planned topology of point P2. The firewall rules has to consider the connection direction between VPN Clients and Servers, given the fact that each Client initiate a connection thoward a Server. In particular the firewall rules must allows inbound traffic for a VPN server, and outbound traffic for a VPN client.
		5. depending on the specific cloud implementation, there could be additional traffic rules to be enabled. As an example, in Microsoft Azure there is the need to enable the "forward between NICs", and in Openstack there is the need to enable "traffic from/to unknown networks". 

	T2. - Configuration of each of the instances of the Network service 
		1. configure the OpenVPN service of each of the Network service instances. This depends on the planning of point P2 (i.e. Server or Client role), and on the planning ofs point P3.
		2. configure the firewall and routing tables for each of the Network service instance. Similarly to what done in point T1.4, the traffic must be allowed as inbound or outbound on WAN interface depending on the server/client role and the topology, but in this case at the level of the WAN network interface of the VM hosting the Network service. Moreover, there has to be a set of routing rules instructing each instance of the Network service to properely route traffic trough the WAN interface (when having sections of the same SUNFISH Tenant in two different clouds), or between different LAN interfaces (when having sections of the same SUNFISH Tenant in the same clouds). As a result, for each SUNFISH Tenant, all the sections composing the Tenant must have full Network OSI visibility, while different SUNFISH sections has to be isolated. An exception is given by the Infrastructure Tenant, which have to be visible to all the remaining SUNFISH sections (see SUNFISH Deliverable D3.11).

===================================
Configuration of a VPN: details
===================================
This sub-section further elaborats the point T2 trough an example. In our prototype we used PFsense 2.4.1 for the the instances of the Network service. The configuration of the services can be done by web interface, or by providing an XML file. In the following, we provide the relevant parts of such XML file for the configuration of an instance of a Network service to be configured as a VPN Client.
We consider an example in which network network N1 located in Cloud "A" has to be aggregated to networks N2, N3 located in Cloud "B".
In particular we have:

	1. Cloud "A" have network N1 defined as 192.168.1.0/24, with an instance of PF-Sense "NS1" to be configured as server
	2. Cloud "A" have networks N2,N3 defined as 192.168.8.0/24 and 192.168.9.0/24, with an instance of PF-Sense "NS2" to be configured as a client 

Then for "NS2" there are the following relevant parts to be customized in its configuration:

-------------------
Network interfaces:
-------------------
There are defined the network interfaces to be used by PF-Sense.

.. code-block:: xml

	<interfaces>
		<wan>
			<enable></enable>
			<if>hn0</if>
			<ipaddr>dhcp</ipaddr>
			<gateway></gateway>
			<blockbogons>on</blockbogons>
			<media></media>
			<mediaopt></mediaopt>
			<dhcp6-duid></dhcp6-duid>
			<dhcp6-ia-pd-len>0</dhcp6-ia-pd-len>
		</wan>
		<lan1>
			<descr><![CDATA[LAN1]]></descr>
			<if>hn1</if>
			<enable></enable>
			<spoofmac></spoofmac>
			<mtu>1446</mtu>
			<ipaddr>192.168.8.254</ipaddr>
			<subnet>24</subnet>
		</lan1>
		<lan2>
			<descr><![CDATA[LAN2]]></descr>
			<if>hn2</if>
			<enable></enable>
			<spoofmac></spoofmac>
			<mtu>1446</mtu>
			<ipaddr>192.168.9.254</ipaddr>
			<subnet>24</subnet>
		</lan2>
	</interfaces>

-------------------	
Firewall rules:
-------------------

It is allowed an administator IP 1.5.5.5 to configure the PF-Sense trough web interface and ssh:

.. code-block:: xml

	<filter>
		<rule>
			<id></id>
			<tracker>1511201327</tracker>
			<type>pass</type>
			<interface>wan</interface>
			<ipprotocol>inet</ipprotocol>
			<tag></tag>
			<tagged></tagged>
			<max></max>
			<max-src-nodes></max-src-nodes>
			<max-src-conn></max-src-conn>
			<max-src-states></max-src-states>
			<statetimeout></statetimeout>
			<statetype><![CDATA[keep state]]></statetype>
			<os></os>
			<protocol>tcp</protocol>
			<source>
				<address>1.5.5.5</address>
			</source>
			<destination>
				<network>wan</network>
				<port>22</port>
			</destination>
			<descr><![CDATA[To Configure PF-Sense. ONLY FOR TESTING. To be CLOSED in real world scenario.]]></descr>
		</rule>
		<rule>
			<id></id>
			<tracker>1511201153</tracker>
			<type>pass</type>
			<interface>wan</interface>
			<ipprotocol>inet</ipprotocol>
			<tag></tag>
			<tagged></tagged>
			<max></max>
			<max-src-nodes></max-src-nodes>
			<max-src-conn></max-src-conn>
			<max-src-states></max-src-states>
			<statetimeout></statetimeout>
			<statetype><![CDATA[keep state]]></statetype>
			<os></os>
			<protocol>tcp</protocol>
			<source>
				<address>1.5.5.5</address>
			</source>
			<destination>
				<network>wan</network>
				<port>443</port>
			</destination>
			<descr><![CDATA[To Configure PF-Sense. ONLY FOR TESTING. To be CLOSED in real world scenario.]]></descr>
		</rule>

It is allowed the PF-Sense client to connect to the PF-Sense Server at 1.2.3.4:8443 :

.. code-block:: xml

		<rule>
			<id></id>
			<tracker>1510931486</tracker>
			<type>pass</type>
			<interface>wan</interface>
			<ipprotocol>inet</ipprotocol>
			<tag></tag>
			<tagged></tagged>
			<max></max>
			<max-src-nodes></max-src-nodes>
			<max-src-conn></max-src-conn>
			<max-src-states></max-src-states>
			<statetimeout></statetimeout>
			<statetype><![CDATA[keep state]]></statetype>
			<os></os>
			<protocol>tcp</protocol>
			<source>
				<network>wan</network>
			</source>
			<destination>
				<address>1.2.3.4</address>
				<port>8443</port>
			</destination>
			<descr><![CDATA[Allow to initiate VPN connection to the VPN Server Cloud]]></descr>
		</rule>

It is allowed traffic from/to the Sections:

.. code-block:: xml

		<rule>
			<id></id>
			<tracker>1510931286</tracker>
			<type>pass</type>
			<interface>openvpn</interface>
			<ipprotocol>inet</ipprotocol>
			<tag></tag>
			<tagged></tagged>
			<max></max>
			<max-src-nodes></max-src-nodes>
			<max-src-conn></max-src-conn>
			<max-src-states></max-src-states>
			<statetimeout></statetimeout>
			<statetype><![CDATA[keep state]]></statetype>
			<os></os>
			<source>
				<any></any>
			</source>
			<destination>
				<any></any>
			</destination>
			<descr><![CDATA[Allow traffic trough virtual interface openVPN]]></descr>
		</rule>
		<rule>
			<id></id>
			<tracker>1510935658</tracker>
			<type>pass</type>
			<interface>lan1</interface>
			<ipprotocol>inet</ipprotocol>
			<tag></tag>
			<tagged></tagged>
			<max></max>
			<max-src-nodes></max-src-nodes>
			<max-src-conn></max-src-conn>
			<max-src-states></max-src-states>
			<statetimeout></statetimeout>
			<statetype><![CDATA[keep state]]></statetype>
			<os></os>
			<source>
				<any></any>
			</source>
			<destination>
				<any></any>
			</destination>
			<descr><![CDATA[Allow traffic of lan1]]></descr>
		</rule>
		<rule>
			<id></id>
			<tracker>1512646679</tracker>
			<type>pass</type>
			<interface>lan2</interface>
			<ipprotocol>inet</ipprotocol>
			<tag></tag>
			<tagged></tagged>
			<max></max>
			<max-src-nodes></max-src-nodes>
			<max-src-conn></max-src-conn>
			<max-src-states></max-src-states>
			<statetimeout></statetimeout>
			<statetype><![Allow traffic of lan2]]></statetype>
			<os></os>
			<protocol>tcp</protocol>
			<source>
				<any></any>
			</source>
			<destination>
				<any></any>
			</destination>
			<descr></descr>
		</rule>
	</filter>

----------------------
OpenVPN configuration 
----------------------

This block contains the configuration of the OpenVPN client. In particular it defined the address:port of the Server, the security parameters (i.e., we choose to use a AES-256-GCM shared key between the Server and the Client), the VPN modality (i.e. site-to-site) and a routing rule for distributing the traffic among the Sections.

.. code-block:: xml

	<openvpn>
		<openvpn-client>
			<auth_user>admin</auth_user>
			<auth_pass>adminPassword</auth_pass>
			<vpnid>1</vpnid>
			<protocol>TCP4</protocol>
			<dev_mode>tun</dev_mode>
			<ipaddr></ipaddr>
			<interface>wan</interface>
			<local_port></local_port>
			<server_addr>1.2.3.4</server_addr>
			<server_port>8443</server_port>
			<proxy_addr></proxy_addr>
			<proxy_port></proxy_port>
			<proxy_authtype>none</proxy_authtype>
			<proxy_user></proxy_user>
			<proxy_passwd></proxy_passwd>
			<description></description>
			<mode>p2p_shared_key</mode>
			<topology>subnet</topology>
			<custom_options>route 192.168.0.0 255.255.0.0</custom_options>
			<shared_key>930C5...SHAREDKEYCONTINUES...</shared_key>
			<crypto>AES-128-CBC</crypto>
			<digest>SHA1</digest>
			<engine>none</engine>
			<tunnel_network>10.10.9.0/24</tunnel_network>
			<tunnel_networkv6></tunnel_networkv6>
			<remote_network>192.168.8.0/24, 192.168.9.0/24</remote_network>
			<remote_networkv6></remote_networkv6>
			<use_shaper></use_shaper>
			<compression></compression>
			<auth-retry-none></auth-retry-none>
			<passtos></passtos>
			<udp_fast_io></udp_fast_io>
			<sndrcvbuf></sndrcvbuf>
			<route_no_pull></route_no_pull>
			<route_no_exec></route_no_exec>
			<verbosity_level>1</verbosity_level>
			<ncp-ciphers>AES-256-GCM,AES-128-GCM</ncp-ciphers>
			<ncp_enable>disabled</ncp_enable>
		</openvpn-client>
	</openvpn>









