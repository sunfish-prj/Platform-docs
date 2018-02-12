####################
Platform components
####################

The SUNFISH Platform is a modular software solution that enables **dynamic and secure creation of cloud federations and their management**. Its components interact between them to establish the FaaS approach but their use is not limited to this as all of them can be deployed independently. 

.. image:: images/platform.png

**************************
Identity Management (IDM) 
**************************

Software providing a set of services to authenticate the access to and within a FaaS federation. It supports the authentication of all the entities part of the federation, varying from users and administrators to service providers and platform components. The added value is that by abstracting the existing IDM solutions to pre-agreed roles in the federation, it enables a flexible definition of data access and data usage policies. This enables the setup of a SUNFISH federation, that uses the pre-deployed IDM solutions. Furthermore, the platform is integrated with eIDAS and it enables a pan-European user authentication.

********************
Data Security (DS)
********************

Software aiming to enforce the access control policies associated with the federated services. Its main role is to decide whether to allow access requests concerning service requests and provisioning. Existing approaches focus on access control or data-usage control. The added value of this component is provided by combining these approaches and rely on developed advanced data protection mechanisms. Extensions are based on well known access control languages. Furthermore, this component intercepts the main communication channel and applies the defined policies, based on data as well as endpoint characteristics.

***********************************************
Federated Administration and Monitoring (FAM)
***********************************************

Software representing the logical entry-points for the management of a FaaS federation, hence, for the interaction with the SUNFISH platform components. It provides a front-end for the administrators and service consumers of the federation based on a graphical web interface. It permits the administration of member clouds (i.e., entering and leaving a federation), tenants (i.e., creation and deploying of tenants), service publishing (i.e., registering a service to the federation), and service provisioning (i.e., management of Service Level Agreement and access control policies).

****************************************
Intelligent Workload Management (IWM)
****************************************

Software performing service brokerage of the federated services offered by the member clouds. In particular, once a service consumer requests a service, it provides an optimal federation-based workload deployment target to satisfy such a service request. To actually deploy the workload, the IWM interacts directly with the clouds to create / delete virtual machines running on the federated clouds. The IWM can provide different workload management strategies optimised according to different parameters. It thus solves an optimisation problem based on the current state of the federation and the requested service. IWM offers a comprehensive set of services as a stand-alone component, with added value being: accent on end-user, pushing more IT governance to the edges, reducing operator's load, integration with blockchain Service Ledger offering higher guarantees against malicious data manipulation.

******************
Data Masking (DM)
******************

Software providing a generic service for masking in a selective way personal and/or sensitive information. This service is called masking service. The service, given a policy and payload (e.g. text, JSON, XML), results with a masked payload. The masked payload itself is identical in format and structure to the original payload except for the personal or sensitive information that is masked. The added value provided by the masking component is its combined support for selecting the sensitive elements and the actions performed (redaction, tokenization, encryption). Moreover, both the selection and action are highly configurable using a flexible policy.

********************
Anonymization (ANM)
********************

Software providing Micro data and Macro data anonymization services.
Micro data anonymization: a data set is released with the k-anonymity guaranty. This process ensures the protection of sensitive information against linkage attacks using other open data sets.
Macro data anonymization: statistical data are released with differential privacy guarantees. This process adds noise to the summary statistic such that the probability to identify if a single person is added or removed is extremely small. The added value of this component is its flexible support for different privacy guarantees when releasing a data set. Specifically the component allows the user to select the required privacy guarantee that best fits the use case: data perturbation, k-anonymity and crowd blending. 

***********************************
Federated Runtime Monitoring (FRM)
***********************************

Software providing a distributed infrastructure to intercept (via transparent plug-in proxies) and monitor every access control request received and possibly authorised by the Data Security (DS). The added value lays in the usage of smart-contracts (programs that facilitate, verify, or respect the negotiation or execution of a contract).

*******************************
Federated Security Audit (FSA)
*******************************

Software providing an automatic detection service against vulnerabilities in the distributed access control system and against security breaches possibly occurred within the federation. The added value of this component is its use of Role Mining techniques to identify real needs from users' behaviour. This allows to identify vulnerabilities and security breaches with much more confidence.

**************************************
Secure Multi-party Computation (SMC) 
**************************************

Software enabling privacy-preserving computation of sensitive data. It can be thus used for computing tasks on confidential data. The added value of this component is that it is integrated with the governmental backbone technology UXP (X-Road) already deployed in Estonia, Finland, Namibia, Haiti, Azerbaijan and ongoing deployment in Ukraine. This is a novel way of securely processing administrative data that enables to use private or public cloud resources.

********************
Service Ledger (SL)
********************

Blockchain-based infrastructure managing the storage and evaluation of governance data. Its seamless integration with the SUNFISH platform via the Service Ledger Interface component permits realising the FaaS "democratic" governance. The Service Ledger offers a set of APIs used by authorised components to invoke smart contracts deployed on the Service Ledger.
