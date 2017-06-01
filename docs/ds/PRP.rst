SUNFISH Policy Retrieval Point (PRP) API
========================================

The PRP is not included in the OASIS XACML standard, but provides
another abstraction level of the PAP

**Version:** 1.0.0

| **Contact information:**
| Alexander Marsalek
| alexander.marsalek@a-sit.at
|

/v1/collect
---------------

*POST*
''''''''''

**Summary:** This endpoint is used by PDPs to retrieve   collection of
policies for   specified decision request.

**Description:**

**Parameters**

+-------+-------------+--------------+-----------+-------+
| Name  | Located in  | Description  | Required  | Schema|
|       |             |              |           |       |
+=======+=============+==============+===========+=======+
| body  | body        | Contains the | Yes       | string|
|       |             | request      |           |       |
|       |             | formatted    |           |       |
|       |             | according to |           |       |
|       |             | the XACML    |           |       |
|       |             | decision     |           |       |
|       |             | request      |           |       |
|       |             | language     |           |       |
|       |             | with all     |           |       |
|       |             | relevant     |           |       |
|       |             | data and     |           |       |
|       |             | attributes   |           |       |
|       |             | necessary    |           |       |
|       |             | for the PRP  |           |       |
|       |             | to identify  |           |       |
|       |             | the relevant |           |       |
|       |             | policies.    |           |       |
+-------+-------------+--------------+-----------+-------+

**Responses**

+-------+--------------+---------+
| Code  | Description  | Schema  |
+=======+==============+=========+
| 200   | Contains     | string  |
|       | single       |         |
|       | policy set   |         |
|       | where all    |         |
|       | policies are |         |
|       | contained or |         |
|       | references   |         |
|       | according to |         |
|       | the XACML    |         |
|       | policy set   |         |
|       | schema.      |         |
+-------+--------------+---------+
| 400   | Invalid      |         |
|       | request      |         |
+-------+--------------+---------+
| 404   | No policies  |         |
|       | matching the |         |
|       | specified    |         |
|       | request were |         |
|       | found        |         |
+-------+--------------+---------+

/v1/policyset/{id}/{version}
--------------------------------

*GET*
'''''''''

**Summary:** This endpoint is used to retrieve   policy by id.
Optionally   version can be specified.

**Description:**

**Parameters**

+---------------+-------------+--------------+-----------+-------+
| Name          | Located in  | Description  | Required  | Schema|
|               |             |              |           |       |
+===============+=============+==============+===========+=======+
| rootPolicySet | query       | true         | false     | Yes   |
|               |             |              | Defines   |       |
|               |             |              | if   root |       |
|               |             |              | policy-se |       |
|               |             |              | t         |       |
|               |             |              | or        |       |
|               |             |              | re-usable |       |
|               |             |              | policies- |       |
|               |             |              | set       |       |
|               |             |              | should be |       |
|               |             |              | returned. |       |
+---------------+-------------+--------------+-----------+-------+
| id            | path        | Specifies    | Yes       | string|
|               |             | the id of    |           |       |
|               |             | the policy   |           |       |
|               |             | set to be    |           |       |
|               |             | returned in  |           |       |
|               |             | the          |           |       |
|               |             | response.    |           |       |
+---------------+-------------+--------------+-----------+-------+
| version       | path        | Specifies    | Yes       | string|
|               |             | the version  |           |       |
|               |             | of the       |           |       |
|               |             | policy set   |           |       |
|               |             | to be        |           |       |
|               |             | returned in  |           |       |
|               |             | the          |           |       |
|               |             | response. If |           |       |
|               |             | no version   |           |       |
|               |             | is specified |           |       |
|               |             | the newest   |           |       |
|               |             | policy set   |           |       |
|               |             | will be      |           |       |
|               |             | returned.    |           |       |
+---------------+-------------+--------------+-----------+-------+

**Responses**

+--------+--------------------------------------------------------+----------+
| Code   | Description                                            | Schema   |
+========+========================================================+==========+
| 200    | Contains the requested policy set                      | string   |
+--------+--------------------------------------------------------+----------+
| 400    | Invalid request                                        |          |
+--------+--------------------------------------------------------+----------+
| 403    | The requestor is not allowed to retrieve this policy   |          |
+--------+--------------------------------------------------------+----------+
| 404    | The policy set with the specified id was not found     |          |
+--------+--------------------------------------------------------+----------+

/v1/policy/{id}
-------------------

*GET*
'''''''''

**Summary:** This endpoint is used to retrieve   policy by id.
Optionally   version can be specified.

**Description:**

**Parameters**

+------------+-------------+--------------+-----------+-------+
| Name       | Located in  | Description  | Required  | Schema|
|            |             |              |           |       |
+============+=============+==============+===========+=======+
| rootPolicy | query       | true         | false     | Yes   |
|            |             |              | Defines   |       |
|            |             |              | if   root |       |
|            |             |              | policy or |       |
|            |             |              |           |       |
|            |             |              | re-usable |       |
|            |             |              | policy    |       |
|            |             |              | should be |       |
|            |             |              | returned. |       |
+------------+-------------+--------------+-----------+-------+
| id         | path        | Specifies    | Yes       | string|
|            |             | the id of    |           |       |
|            |             | the policy   |           |       |
|            |             | to be        |           |       |
|            |             | returned in  |           |       |
|            |             | the          |           |       |
|            |             | response.    |           |       |
+------------+-------------+--------------+-----------+-------+

**Responses**

+--------+--------------------------------------------------------+----------+
| Code   | Description                                            | Schema   |
+========+========================================================+==========+
| 200    | Contains the requested policy set                      | string   |
+--------+--------------------------------------------------------+----------+
| 400    | Invalid request                                        |          |
+--------+--------------------------------------------------------+----------+
| 403    | The requestor is not allowed to retrieve this policy   |          |
+--------+--------------------------------------------------------+----------+
| 404    | The policy set with the specified id was not found     |          |
+--------+--------------------------------------------------------+----------+

/v1/policyset/{id}
----------------------

*GET*
'''''''''

**Summary:** This endpoint is used to retrieve   policy by id.
Optionally   version can be specified.

**Description:**

**Parameters**

+---------------+-------------+--------------+-----------+-------+
| Name          | Located in  | Description  | Required  | Schema|
|               |             |              |           |       |
+===============+=============+==============+===========+=======+
| rootPolicySet | query       | true         | false     | Yes   |
|               |             |              | Defines   |       |
|               |             |              | if   root |       |
|               |             |              | policy-se |       |
|               |             |              | t         |       |
|               |             |              | or        |       |
|               |             |              | re-usable |       |
|               |             |              | policies- |       |
|               |             |              | set       |       |
|               |             |              | should be |       |
|               |             |              | returned. |       |
+---------------+-------------+--------------+-----------+-------+
| id            | path        | Specifies    | Yes       | string|
|               |             | the id of    |           |       |
|               |             | the policy   |           |       |
|               |             | set to be    |           |       |
|               |             | returned in  |           |       |
|               |             | the          |           |       |
|               |             | response.    |           |       |
+---------------+-------------+--------------+-----------+-------+

**Responses**

+--------+--------------------------------------------------------+----------+
| Code   | Description                                            | Schema   |
+========+========================================================+==========+
| 200    | Contains the requested policy set                      | string   |
+--------+--------------------------------------------------------+----------+
| 400    | Invalid request                                        |          |
+--------+--------------------------------------------------------+----------+
| 403    | The requestor is not allowed to retrieve this policy   |          |
+--------+--------------------------------------------------------+----------+
| 404    | The policy set with the specified id was not found     |          |
+--------+--------------------------------------------------------+----------+

/v1/policy/{id}/{version}
-----------------------------

*GET*
'''''''''

**Summary:** This endpoint is used to retrieve   policy by id.
Optionally   version can be specified.

**Description:**

**Parameters**

+------------+-------------+--------------+-----------+-------+
| Name       | Located in  | Description  | Required  | Schema|
|            |             |              |           |       |
+============+=============+==============+===========+=======+
| rootPolicy | query       | true         | false     | Yes   |
|            |             |              | Defines   |       |
|            |             |              | if   root |       |
|            |             |              | policy or |       |
|            |             |              |           |       |
|            |             |              | re-usable |       |
|            |             |              | policy    |       |
|            |             |              | should be |       |
|            |             |              | returned. |       |
+------------+-------------+--------------+-----------+-------+
| id         | path        | Specifies    | Yes       | string|
|            |             | the id of    |           |       |
|            |             | the policy   |           |       |
|            |             | to be        |           |       |
|            |             | returned in  |           |       |
|            |             | the          |           |       |
|            |             | response.    |           |       |
+------------+-------------+--------------+-----------+-------+
| version    | path        | Specifies    | Yes       | string|
|            |             | the version  |           |       |
|            |             | of the       |           |       |
|            |             | policy to be |           |       |
|            |             | returned in  |           |       |
|            |             | the          |           |       |
|            |             | response. If |           |       |
|            |             | no version   |           |       |
|            |             | is specified |           |       |
|            |             | the newest   |           |       |
|            |             | policy will  |           |       |
|            |             | be returned. |           |       |
+------------+-------------+--------------+-----------+-------+

**Responses**

+--------+--------------------------------------------------------+----------+
| Code   | Description                                            | Schema   |
+========+========================================================+==========+
| 200    | Contains the requested policy set                      | string   |
+--------+--------------------------------------------------------+----------+
| 400    | Invalid request                                        |          |
+--------+--------------------------------------------------------+----------+
| 403    | The requestor is not allowed to retrieve this policy   |          |
+--------+--------------------------------------------------------+----------+
| 404    | The policy set with the specified id was not found     |          |
+--------+--------------------------------------------------------+----------+
