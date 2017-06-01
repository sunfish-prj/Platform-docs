SUNFISH Policy Decision Point (PDP) API
=======================================

This API is primarily used by adjacent PEPs to issue authorization
requests for intra-zone and cross-zone interactions. In this
specification we partially rely on the REST profile suggested by the
OASIS XACML Standard

**Version:** 1.0.0

| **Contact information:**
| Bernd Prünster
| bernd.pruenster@a-sit.at
|

/v1
-------

*GET*
'''''''''

**Summary:** API entry point. This point is used to identify
functionality and endpoints provided by PDP.

**Description:**

**Parameters**

+--------+--------------+---------------+------------+----------+
| Name   | Located in   | Description   | Required   | Schema   |
+========+==============+===============+============+==========+
+--------+--------------+---------------+------------+----------+

**Responses**

+-------+--------------+
| Code  | Description  |
+=======+==============+
| 200   | The response |
|       | contains a   |
|       | resource     |
|       | with link    |
|       | relation     |
|       | http://docs. |
|       | oasis-open.o |
|       | rg/ns/xacml/ |
|       | relation/pdp |
|       | and a valid  |
|       | URL.         |
+-------+--------------+


/v1/verifyServicePolicy
---------------------------

*POST*
''''''''''

**Summary:** Verify a service policy

**Description:**

**Parameters**

+-------------------+-------------+--------------+-----------+-------+
| Name              | Located in  | Description  | Required  | Schema|
|                   |             |              |           |       |
+===================+=============+==============+===========+=======+
| SUNFISH-signature | header      | This field   | No        | string|
|                   |             | is used to   |           |       |
|                   |             | provide      |           |       |
|                   |             | integrity    |           |       |
|                   |             | and          |           |       |
|                   |             | authenticity |           |       |
|                   |             | of messages. |           |       |
+-------------------+-------------+--------------+-----------+-------+
| body              | body        | Contains     | Yes       | string|
|                   |             | XACML-format |           |       |
|                   |             | ted          |           |       |
|                   |             | policy for   |           |       |
|                   |             | PDP to       |           |       |
|                   |             | perform      |           |       |
|                   |             | verification.|           |       |
|                   |             |              |           |       |
+-------------------+-------------+--------------+-----------+-------+

**Responses**

+--------+-------------------------------------------------------+------------------------------------------------+
| Code   | Description                                           | Schema                                         |
+========+=======================================================+================================================+
| 200    | Contains information about the verification result.   | `VerifyPolicyResult <#verifyPolicyResult>`__   |
+--------+-------------------------------------------------------+------------------------------------------------+
| 400    | Invalid request                                       |                                                |
+--------+-------------------------------------------------------+------------------------------------------------+
| 404    | The requestor is not allowed                          |                                                |
+--------+-------------------------------------------------------+------------------------------------------------+

/v1/verifyServicePolicySet
------------------------------

*POST*
''''''''''

**Summary:** Verify a service policy set

**Description:**

**Parameters**

+---------------------+-------------+--------------+-----------+-------+
| Name                | Located in  | Description  | Required  | Schema|
|                     |             |              |           |       |
+=====================+=============+==============+===========+=======+
| SUNFISH-signature   | header      | This field   | No        | string|
|                     |             | is used to   |           |       |
|                     |             | provide      |           |       |
|                     |             | integrity    |           |       |
|                     |             | and          |           |       |
|                     |             | authenticity |           |       |
|                     |             | of messages. |           |       |
+---------------------+-------------+--------------+-----------+-------+
| body                | body        | Contains     | Yes       | string|
|                     |             | XACML-format |           |       |
|                     |             | ted          |           |       |
|                     |             | policy set   |           |       |
|                     |             | for PDP to   |           |       |
|                     |             | perform      |           |       |
|                     |             | verification |           |       |
|                     |             | .            |           |       |
+---------------------+-------------+--------------+-----------+-------+

**Responses**

+--------+-------------------------------------------------------+------------------------------------------------+
| Code   | Description                                           | Schema                                         |
+========+=======================================================+================================================+
| 200    | Contains information about the verification result.   | `VerifyPolicyResult <#verifyPolicyResult>`__   |
+--------+-------------------------------------------------------+------------------------------------------------+
| 400    | Invalid request                                       |                                                |
+--------+-------------------------------------------------------+------------------------------------------------+
| 404    | The requestor is not allowed                          |                                                |
+--------+-------------------------------------------------------+------------------------------------------------+

/v1/authorization
---------------------

*POST*
''''''''''

**Summary:** This endpoint is used by PEPs to issue authorization
decision requests to PDP. These requests are sent using POST method.
Inputs to this endpoint are parameters that describe access requests
initiated by entities interacting through the calling PEP. Additionally,
this request contains other contextual parameters that can be used by
PDP to evaluate request.

**Description:**

**Parameters**

+---------------------+-------------+--------------+-----------+-------+
| Name                | Located in  | Description  | Required  | Schem |
|                     |             |              |           | a     |
+=====================+=============+==============+===========+=======+
| SUNFISH-signature   | header      | This field   | No        | string|
|                     |             | is used to   |           |       |
|                     |             | provide      |           |       |
|                     |             | integrity    |           |       |
|                     |             | and          |           |       |
|                     |             | authenticity |           |       |
|                     |             | of messages. |           |       |
+---------------------+-------------+--------------+-----------+-------+
| body                | body        | Contains     | Yes       | string|
|                     |             | XACML-format |           |       |
|                     |             | ted          |           |       |
|                     |             | (or other)   |           |       |
|                     |             | request with |           |       |
|                     |             | all relevant |           |       |
|                     |             | data and     |           |       |
|                     |             | attributes   |           |       |
|                     |             | necessary    |           |       |
|                     |             | for PDP to   |           |       |
|                     |             | perform      |           |       |
|                     |             | authorization|           |       |
|                     |             | decision.    |           |       |
|                     |             |              |           |       |
+---------------------+-------------+--------------+-----------+-------+

**Responses**

+-------+--------------+---------+
| Code  | Description  | Schema  |
+=======+==============+=========+
| 200   | Contains     | string  |
|       | complete     |         |
|       | XACML-format |         |
|       | ted          |         |
|       | answer. Body |         |
|       | can include  |         |
|       | additional   |         |
|       | answer that  |         |
|       | deals with   |         |
|       | activity     |         |
|       | context, if  |         |
|       | requested.   |         |
+-------+--------------+---------+
| 400   | Invalid      |         |
|       | XACML        |         |
|       | request      |         |
+-------+--------------+---------+
| 404   | Requestor is |         |
|       | not allowed  |         |
|       | to perform   |         |
|       | the request  |         |
+-------+--------------+---------+

Models
----------

\ **VerifyPolicyResult**

+---------------+---------+--------------+-----------+
| Name          | Type    | Description  | Required  |
+===============+=========+==============+===========+
| status        | string  | Indicates    | No        |
|               |         | the status   |           |
|               |         | of the       |           |
|               |         | verification |           |
|               |         | operation.   |           |
+---------------+---------+--------------+-----------+
| description   | string  | Description, | No        |
|               |         | containing   |           |
|               |         | detailed     |           |
|               |         | information  |           |
|               |         | about the    |           |
|               |         | requested    |           |
|               |         | operation.   |           |
+---------------+---------+--------------+-----------+
| statusCode    | integer | Status code  | No        |
|               |         | of the       |           |
|               |         | operation.   |           |
+---------------+---------+--------------+-----------+
