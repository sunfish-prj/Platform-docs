Policy Information Point (PIP) 
===============================

The PIP is generally defined as “the system entity that acts as   source
of attribute values

**Version:** 1.0.0

| **Contact information:**
| Dominik Ziegler
| dominik.ziegler@a-sit.at
|

/v1/collect
---------------

*GET*
'''''''''

**Summary:** This endpoint is used to retrieve   collection of all
available attribute ids

**Description:**

**Parameters**

+----------------+-------------+--------------+-----------+-------+
| Name           | Located in  | Description  | Required  | Schema|
|                |             |              |           |       |
+================+=============+==============+===========+=======+
| SUNFISH-issuer | header      | References   | Yes       | string|
|                |             | the entity   |           |       |
|                |             | that issued  |           |       |
|                |             | the request. |           |       |
|                |             | This field   |           |       |
|                |             | includes the |           |       |
|                |             | data that    |           |       |
|                |             | confirms the |           |       |
|                |             | authenticati |           |       |
|                |             | on           |           |       |
|                |             | of source    |           |       |
|                |             | entity and   |           |       |
|                |             | its          |           |       |
|                |             | authenticati |           |       |
|                |             | on           |           |       |
|                |             | level.       |           |       |
+----------------+-------------+--------------+-----------+-------+

**Responses**

+-------+--------------+---------+
| Code  | Description  | Schema  |
+=======+==============+=========+
| 200   | Contains     | string  |
|       | collection   |         |
|       | of attribute |         |
|       | designators  |         |
|       | ids          |         |
|       | according to |         |
|       | the          |         |
|       | attribute    |         |
|       | designator   |         |
|       | set schema.  |         |
+-------+--------------+---------+
| 400   | Invalid      |         |
|       | request      |         |
+-------+--------------+---------+
| 403   | The          |         |
|       | requestor is |         |
|       | not allowed  |         |
+-------+--------------+---------+

/v1/request
---------------

*POST*
''''''''''

**Summary:** This endpoint is used to retrieve additional attributes

**Description:**

**Parameters**

+----------------+-------------+--------------+-----------+-------+
| Name           | Located in  | Description  | Required  | Schema|
|                |             |              |           |       |
+================+=============+==============+===========+=======+
| body           | body        | Contains the | No        | string|
|                |             | requested    |           |       |
|                |             | attributes   |           |       |
|                |             | and the      |           |       |
|                |             | request      |           |       |
|                |             | context as   |           |       |
|                |             | issued by    |           |       |
|                |             | the PEP. If  |           |       |
|                |             | multiple     |           |       |
|                |             | PIPs are     |           |       |
|                |             | involved,    |           |       |
|                |             | the PIP      |           |       |
|                |             | always       |           |       |
|                |             | receive the  |           |       |
|                |             | most recent  |           |       |
|                |             | request      |           |       |
|                |             | context.     |           |       |
+----------------+-------------+--------------+-----------+-------+
| SUNFISH-issuer | header      | References   | Yes       | string|
|                |             | the entity   |           |       |
|                |             | that issued  |           |       |
|                |             | the request. |           |       |
|                |             | This field   |           |       |
|                |             | includes the |           |       |
|                |             | data that    |           |       |
|                |             | confirms the |           |       |
|                |             | authenticati |           |       |
|                |             | on           |           |       |
|                |             | of source    |           |       |
|                |             | entity and   |           |       |
|                |             | its          |           |       |
|                |             | authenticati |           |       |
|                |             | on           |           |       |
|                |             | level.       |           |       |
+----------------+-------------+--------------+-----------+-------+

**Responses**

+-------+--------------+---------+
| Code  | Description  | Schema  |
+=======+==============+=========+
| 200   | The request  | string  |
|       | context was  |         |
|       | enhanced     |         |
|       | with all or  |         |
|       | some of the  |         |
|       | requested    |         |
|       | attributes.  |         |
+-------+--------------+---------+
| 400   | Invalid      |         |
|       | request      |         |
+-------+--------------+---------+
| 403   | The          |         |
|       | requestor is |         |
|       | not allowed  |         |
+-------+--------------+---------+
| 404   | This PIP     |         |
|       | does not     |         |
|       | provide any  |         |
|       | of the       |         |
|       | requested    |         |
|       | attributes.  |         |
+-------+--------------+---------+
