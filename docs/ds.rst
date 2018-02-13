.. _ds-label:

##################
Data Security (DS)
##################

The SUNFISH Data Security (DS) enforcement infrastructure is responsible for regulating and securing access to services in a federated cloud environment.

The infrastructure consists of the following components:

 * The **Policy Enforcement Gateway** (PEG) responsible for enforcing decision regarding whether access to a resource is granted or not and which obligations need to be observed (if any). This component serves as the main entry point to a service protected by the SUNFISH DS enforcement infrastructure.
 * An accompanying **Proxy** enabling the non-SUNFISH-aware applications to utilise the benefits of the SUNFISH platform.
 * A **Policy Decision Point** (PDP) evaluating a decision request, indicating whether access to a service should be granted or not. In addition, Obligations such as *data masking*, for example can be part of the decision.
 * **Policy Information Points** (PIPs) delivering information to the PEG and the PDP to enhance decision requests.
 * A **Policy Administration Point** (PAP) providing an interface for administration data security policies.
 * The **Service Ledger Interface** responsible for storing, managing and delivering policies to the PDP for evaluation. Setup an operation of the SLI is described separately, since it is operated independently of the other components.
 * A **Masking Service** providing *data masking* capabilities to the PEG. Like the SLI, the masking service is operated independently of the other components and therefore also discussed separately.

Typically, the enforcement infrastructure will be deployed among different *tenants*. A minimal example consists of one *infrastructure tenant* and a *service tenant*. A typical cross-cloud set-up will be as follows, with highlighted typical inter-tenant interactions.

.. image:: images/ds.pdf

The *infrastructure tenant* will  house the PDP, any number of PIPs and the PRP. The *service tenant* hosts the actual service to be protected by the enforcement infrastructure as well as the PEG, any number of PIPs and the proxy to maintain backwards compatibility to non-SUNFISH-aware clients. 

As outlined initially, the PEG located at the service tenant serves as the main entry point, responding to incoming requests, which can either be submitted directly to the PEG, or through the proxy. In case the requests was directed to the proxy, responses are also interpreted by the proxy and reduced in such a way that non-SUNFISH-aware applications are able to interpret it correctly (albeit losing expressibility in the process).


