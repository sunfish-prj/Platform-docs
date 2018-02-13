##################
Data Masking (DM)
##################

The masking service provides the means by which sensitive data is replaced, possibly in a reversible and format preserving manner, with data that is unintelligible to receiver. The service supports parsing and classifying sensitive/confidential data in payloads, mask them in different ways and supports flexible configuration of the process.

================
Functionality
================

The masking service provided the necessary functionality to mask different data payloads (e.g. xml, json etc). The service enables to user to upload a policy which configures where the data items can be found in the payload (e.g. xpath, css etc.) and how to mask them (e.g. redaction, format preserving encryption). The Masking flow is very simple:

1.	Upload policy
2.	Create context
3.	Process payload (mask/unmask)

All the functionalities are invoked via RESTful calls. Examples are provided in the deployment guide.