SUNFISH Policy Administration Point (PAP) API
=============================================

The PAP interface follows a straight forward REST interface, as it
requires bare access to the policy storage.

**Version:** 1.0.0

| **Contact information:**
| Alexander Marsalek
| alexander.marsalek@a-sit.at
|

/v1/policies
----------------

*GET*
'''''''''

**Summary:** This endpoint is used by entities interfacing with the PAP
to retrieve policies

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
|                |             | may include  |           |       |
|                |             | the data     |           |       |
|                |             | that         |           |       |
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
| 200   | The body of  | string  |
|       | the response |         |
|       | contains the |         |
|       | requested    |         |
|       | policies     |         |
|       | according to |         |
|       | the schema   |         |
|       | defined in   |         |
|       | Listing 3.   |         |
|       | The response |         |
|       | result set   |         |
|       | only         |         |
|       | contains a   |         |
|       | certain      |         |
|       | amount of    |         |
|       | entries.     |         |
|       | Pagination   |         |
|       | is done      |         |
|       | using the    |         |
|       | Web Linking  |         |
|       | approach     |         |
|       | according to |         |
|       | RFC5988. A   |         |
|       | Link header  |         |
|       | is included  |         |
|       | in the       |         |
|       | response     |         |
|       | pointing to  |         |
|       | the next     |         |
|       | resultset:   |         |
|       | Link:        |         |
|       | https://%3Ch |         |
|       | ost/pap/api/ |         |
|       | v1/policies/ |         |
|       | ?page=2>;    |         |
|       | rel="next"   |         |
|       | The possible |         |
|       | “rel” values |         |
|       | are "next"   |         |
|       | pointing to  |         |
|       | the next     |         |
|       | result-set.  |         |
|       | Pagination   |         |
|       | URLs are not |         |
|       | allowed to   |         |
|       | be           |         |
|       | constructed  |         |
|       | manually.    |         |
+-------+--------------+---------+
| 400   | Invalid      |         |
|       | request      |         |
+-------+--------------+---------+
| 403   | The          |         |
|       | requestor is |         |
|       | not allowed  |         |
|       | to perform   |         |
|       | this         |         |
|       | operation    |         |
+-------+--------------+---------+
| 404   | No policies  |         |
|       | matching the |         |
|       | specified    |         |
|       | request were |         |
|       | found        |         |
+-------+--------------+---------+

*POST*
''''''''''

**Summary:** This endpoint is used by entities interfacing with the PAP
to add a policy

**Description:**

**Parameters**

+----------------+-------------+--------------+-----------+-------+
| Name           | Located in  | Description  | Required  | Schema|
|                |             |              |           |       |
+================+=============+==============+===========+=======+
| body           | body        | The body of  | Yes       | string|
|                |             | the request  |           |       |
|                |             | contains a   |           |       |
|                |             | to be added  |           |       |
|                |             | policy       |           |       |
|                |             | according to |           |       |
|                |             | the schema   |           |       |
|                |             | in Listing   |           |       |
|                |             | 1.           |           |       |
+----------------+-------------+--------------+-----------+-------+
| SUNFISH-issuer | header      | References   | Yes       | string|
|                |             | the entity   |           |       |
|                |             | that issued  |           |       |
|                |             | the request. |           |       |
|                |             | This field   |           |       |
|                |             | may include  |           |       |
|                |             | the data     |           |       |
|                |             | that         |           |       |
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

+--------+----------------------------------------------------------+----------+
| Code   | Description                                              | Schema   |
+========+==========================================================+==========+
| 200    | Created successful                                       | string   |
+--------+----------------------------------------------------------+----------+
| 400    | Invalid request                                          |          |
+--------+----------------------------------------------------------+----------+
| 403    | The requestor is not allowed to perform this operation   |          |
+--------+----------------------------------------------------------+----------+
| 409    | The policy exists already                                |          |
+--------+----------------------------------------------------------+----------+

*PUT*
'''''''''

**Summary:** This endpoint is used by entities interfacing with the PAP
to update a policy

**Description:**

**Parameters**

+----------------+-------------+--------------+-----------+-------+
| Name           | Located in  | Description  | Required  | Schema|
|                |             |              |           |       |
+================+=============+==============+===========+=======+
| body           | body        | The body of  | Yes       | string|
|                |             | the request  |           |       |
|                |             | contains a   |           |       |
|                |             | to be added  |           |       |
|                |             | policy       |           |       |
|                |             | according to |           |       |
|                |             | the schema   |           |       |
|                |             | in Listing   |           |       |
|                |             | 1.           |           |       |
+----------------+-------------+--------------+-----------+-------+
| SUNFISH-issuer | header      | References   | Yes       | string|
|                |             | the entity   |           |       |
|                |             | that issued  |           |       |
|                |             | the request. |           |       |
|                |             | This field   |           |       |
|                |             | may include  |           |       |
|                |             | the data     |           |       |
|                |             | that         |           |       |
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
| 200   | The body of  | string  |
|       | the request  |         |
|       | contains a   |         |
|       | to be        |         |
|       | updated      |         |
|       | policy       |         |
|       | according to |         |
|       | the schema   |         |
|       | in Listing   |         |
|       | 1.           |         |
+-------+--------------+---------+
| 400   | Invalid      |         |
|       | request      |         |
+-------+--------------+---------+
| 403   | The          |         |
|       | requestor is |         |
|       | not allowed  |         |
|       | to perform   |         |
|       | this         |         |   
|       | operation    |         |
+-------+--------------+---------+
| 404   | Policy to    |         |
|       | update not   |         |
|       | found        |         |
+-------+--------------+---------+

/v1/policies/{id}/{version}
-------------------------------

*DELETE*
''''''''''''

**Summary:** This endpoint is used by entities to remove policies

**Description:**

**Parameters**

+----------------+-------------+--------------+-----------+-------+
| Name           | Located in  | Description  | Required  | Schem |
|                |             |              |           | a     |
+================+=============+==============+===========+=======+
| id             | path        | Id of the    | Yes       | string|
|                |             | policy to    |           |       |
|                |             | delete       |           |       |
+----------------+-------------+--------------+-----------+-------+
| version        | path        | Specifies    | Yes       | string|
|                |             | the version  |           |       |
|                |             | of the       |           |       |
|                |             | policy to be |           |       |
|                |             | deleted      |           |       |
+----------------+-------------+--------------+-----------+-------+
| SUNFISH-issuer | header      | References   | Yes       | string|
|                |             | the entity   |           |       |
|                |             | that issued  |           |       |
|                |             | the request. |           |       |
|                |             | This field   |           |       |
|                |             | may include  |           |       |
|                |             | the data     |           |       |
|                |             | that         |           |       |
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

+--------+----------------------------------------------------------+----------+
| Code   | Description                                              | Schema   |
+========+==========================================================+==========+
| 200    | Deleted successful                                       | string   |
+--------+----------------------------------------------------------+----------+
| 400    | Invalid request                                          |          |
+--------+----------------------------------------------------------+----------+
| 403    | The requestor is not allowed to perform this operation   |          |
+--------+----------------------------------------------------------+----------+
| 404    | Policy not found                                         |          |
+--------+----------------------------------------------------------+----------+
