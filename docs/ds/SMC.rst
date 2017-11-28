SUNFISH Secure Multi-party Computation (SMC) API
================================================

SUNFISH framework's SMC service consists of two components, SMC Proxy and computation node, both providing their own API. In each case, both the request and response body are JSON-encoded

**Version:** 0.1

| **Contact information:**
| Riivo Talviste
| riivo.talviste@cyber.ee
|

SMC Node Service
----------------

SMC Node Service provides the interface to run privacy-preserving programs (i.e., SecreC programs) on SMC nodes. Each SMC node provides this service independently, the client must invoke this service in parallel at each SMC node.

.. swaggerv2doc:: ../../specs/smc.json

SMC Proxy Service
-----------------

SMC Proxy Service is a client-side helper service for secret-sharing user input for the secure multi-party computation and reconstructing the results for further processing in the client application.

.. swaggerv2doc:: ../../specs/smc-proxy.json