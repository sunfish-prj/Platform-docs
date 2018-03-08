Policy Enforcement Point (PEP) 
===============================

The interactions executed inside one zone are checked by and enforced in
the scope of a PEP assigned for that zone. The approach is similar for
the zones that consist of geographically dispersed locations: each PEP
(or sub-PEP) is responsible for its geographical unit or layer. Being
the single point of contact of a zone, the PEP is primarily responsible
for checking incoming and outgoing requests. In the second instance,
depending on security settings and application requirements, PEP might
serve as an inter-zone communication gateway, as well.

**Version:** 1.0.0

| **Contact information:**
| Dominik Ziegler
| dominik.ziegler@a-sit.at
|


/v1/request
---------------

*POST*
''''''''''

**Summary:** This endpoint is used by PEPs to POST new requests to other
PEPs. Inputs to this endpoint are contextual parameters that establish
the request, application and target specific settings. The response of
this action is the data record that contains request id and data
structure describing status parameters or other PEP requirements.

**Description:**

**Parameters**

+-----------------+-------------+--------------+-----------+-------+
| Name            | Located in  | Description  | Required  | Schema|
|                 |             |              |           |       |
+=================+=============+==============+===========+=======+
| body            | body        | Body of the  | No        |[byte] |
|                 |             | original     |           |       |
|                 |             | request      |           |       |
+-----------------+-------------+--------------+-----------+-------+
| SUNFISH-issuer  | header      | References   | No        | string|
|                 |             | the          |           |       |
|                 |             | application  |           |       |
|                 |             | that issued  |           |       |
|                 |             | the request. |           |       |
|                 |             | This field   |           |       |
|                 |             | may include  |           |       |
|                 |             | the data     |           |       |
|                 |             | required to  |           |       |
|                 |             | perform      |           |       |
|                 |             | application  |           |       |
|                 |             | authenticati |           |       |
|                 |             | on,          |           |       |
|                 |             | in the form  |           |       |
|                 |             | of           |           |       |
|                 |             | authenticati |           |       |
|                 |             | on           |           |       |
|                 |             | token.       |           |       |
+-----------------+-------------+--------------+-----------+-------+
| SUNFISH-service | header      | Machine-read | Yes       | string|
|                 |             | able         |           |       |
|                 |             | description  |           |       |
|                 |             | of endpoint  |           |       |
|                 |             | including at |           |       |
|                 |             | least an     |           |       |
|                 |             | identifier   |           |       |
|                 |             | of the       |           |       |
|                 |             | service.     |           |       |
|                 |             | With the     |           |       |
|                 |             | service id,  |           |       |
|                 |             | the PEP can  |           |       |
|                 |             | resolve      |           |       |
|                 |             | other        |           |       |
|                 |             | required     |           |       |
|                 |             | attributes.  |           |       |
+-----------------+-------------+--------------+-----------+-------+
| SUNFISH-request | header      | Machine-read | Yes       | string|
|                 |             | able         |           |       |
|                 |             | description  |           |       |
|                 |             | of the       |           |       |
|                 |             | target       |           |       |
|                 |             | endpoint and |           |       |
|                 |             | request      |           |       |
|                 |             | data. The    |           |       |
|                 |             | PEP at least |           |       |
|                 |             | requires the |           |       |
|                 |             | parameters   |           |       |
|                 |             | method,      |           |       |
|                 |             | port, path   |           |       |
|                 |             | and          |           |       |
|                 |             | protocol. If |           |       |
|                 |             | additional   |           |       |
|                 |             | attributes   |           |       |
|                 |             | are          |           |       |
|                 |             | registered   |           |       |
|                 |             | in the       |           |       |
|                 |             | SUNFISH      |           |       |
|                 |             | federation,  |           |       |
|                 |             | the PEP can  |           |       |
|                 |             | retrieve     |           |       |
|                 |             | these        |           |       |
|                 |             | attributes   |           |       |
|                 |             | from a       |           |       |
|                 |             | correspondin |           |       |
|                 |             |              |           |       |
|                 |             | PIP.         |           |       |
|                 |             | Furthermore, |           |       |
|                 |             | this field   |           |       |
|                 |             | may include  |           |       |
|                 |             | validity     |           |       |
|                 |             | constraints  |           |       |
|                 |             | on a request |           |       |
|                 |             | (not-valid-b |           |       |
|                 |             | efore,       |           |       |
|                 |             | not          |           |       |
|                 |             | valid-after) |           |       |
|                 |             | .            |           |       |
+----------------------------+-------------+--------------+-----------+-------+
| SUNFISH-request-parameters | header      | The          | No        | string|
|                            |             | parameters   |           |       |
|                            |             | related to   |           |       |
|                            |             | the request, |           |       |
|                            |             | including    |           |       |
|                            |             | its          |           |       |
|                            |             | priority,    |           |       |
|                            |             | SLA          |           |       |
|                            |             | requirements |           |       |
|                            |             | ,            |           |       |
|                            |             | call-back    |           |       |
|                            |             | URI. This    |           |       |
|                            |             | field        |           |       |
|                            |             | includes     |           |       |
|                            |             | other        |           |       |
|                            |             | request      |           |       |
|                            |             | meta-data    |           |       |
|                            |             | that may     |           |       |
|                            |             | extend or    |           |       |
|                            |             | override the |           |       |
|                            |             | definitions  |           |       |
|                            |             | provided in  |           |       |
|                            |             | centralized  |           |       |
|                            |             | administrati |           |       |
|                            |             | ve           |           |       |
|                            |             | console.     |           |       |
|                            |             | These        |           |       |
|                            |             | include      |           |       |
|                            |             | request      |           |       |
|                            |             | type,        |           |       |
|                            |             | application- |           |       |
|                            |             | specific     |           |       |
|                            |             | policies or  |           |       |
|                            |             | obligations  |           |       |
|                            |             | to be        |           |       |
|                            |             | applied      |           |       |
|                            |             | beyond the   |           |       |
|                            |             | ones defined |           |       |
|                            |             | in the       |           |       |
|                            |             | central      |           |       |
|                            |             | console, or  |           |       |
|                            |             | parameters   |           |       |
|                            |             | related to   |           |       |
|                            |             | data-masking |           |       |
|                            |             | policies.    |           |       |
|                            |             | The scope of |           |       |
|                            |             | applicable   |           |       |
|                            |             | and allowed  |           |       |
|                            |             | definitions  |           |       |
|                            |             | provided in  |           |       |
|                            |             | this         |           |       |
|                            |             | variable     |           |       |
|                            |             | depends on   |           |       |
|                            |             | an extent of |           |       |
|                            |             | delegation   |           |       |
|                            |             | policies, as |           |       |
|                            |             | determined   |           |       |
|                            |             | in           |           |       |
|                            |             | centralized  |           |       |
|                            |             | console.     |           |       |
+----------------------------+-------------+--------------+-----------+-------+
| SUNFISH-request-data       | header      | This field   | Yes       | string|
|                            |             | encapsulates |           |       |
|                            |             | the original |           |       |
|                            |             | header data  |           |       |
|                            |             | and the      |           |       |
|                            |             | original     |           |       |
|                            |             | query string |           |       |
|                            |             | as issued by |           |       |
|                            |             | the          |           |       |
|                            |             | application. |           |       |
+----------------------------+-------------+--------------+-----------+-------+
| SUNFISH-signature          | header      | This         | No        | string|
|                            |             | parameter is |           |       |
|                            |             | used to      |           |       |
|                            |             | ensure       |           |       |
|                            |             | integrity    |           |       |
|                            |             | and          |           |       |
|                            |             | authenticity |           |       |
|                            |             | of the       |           |       |
|                            |             | source       |           |       |
|                            |             | message for  |           |       |
|                            |             | applications |           |       |
|                            |             | which        |           |       |
|                            |             | require a    |           |       |
|                            |             | higher       |           |       |
|                            |             | degree of    |           |       |
|                            |             | security. It |           |       |
|                            |             | contains     |           |       |
|                            |             | signed       |           |       |
|                            |             | request and  |           |       |
|                            |             | fields,      |           |       |
|                            |             | according to |           |       |
|                            |             | predefined   |           |       |
|                            |             | schema       |           |       |
+----------------------------+-------------+--------------+-----------+-------+
| SUNFISH-activity-context   | header      | Includes the | No        | string|
|                            |             | signed PDP   |           |       |
|                            |             | decision     |           |       |
|                            |             | aimed at     |           |       |
|                            |             | related      |           |       |
|                            |             | audience     |           |       |
|                            |             | (both PEPs   |           |       |
|                            |             | taking part  |           |       |
|                            |             | in           |           |       |
|                            |             | transaction) |           |       |
|                            |             | in the case  |           |       |
|                            |             | where        |           |       |
|                            |             | activity     |           |       |
|                            |             | context is   |           |       |
|                            |             | applied to   |           |       |
|                            |             | allow access |           |       |
|                            |             | decisions to |           |       |
|                            |             | span across  |           |       |
|                            |             | several      |           |       |
|                            |             | interactions |           |       |
|                            |             | and          |           |       |
|                            |             | entities.    |           |       |
+----------------------------+-------------+--------------+-----------+-------+

**Responses**

+--------+--------------------------------+------------+
| Code   | Description                    | Schema     |
+========+================================+============+
| 200    | Body of the original request   | [ byte ]   |
+--------+--------------------------------+------------+

/v1/app-request
-------------------

*POST*
''''''''''

**Summary:** Applications can POST new requests to this endpoint. Inputs
to this endpoint are contextual parameters that establish the request,
application and target specific settings. For this specification, the
applications rely on common SUNFISH functionalities and components. The
response of this action is the original response of the target service
(synchronous use case).

**Description:**

**Parameters**

+----------------------------+-------------+--------------+-----------+-------+
| Name                       | Located in  | Description  | Required  | Schema|
|                            |             |              |           |       |
+============================+=============+==============+===========+=======+
| body                       | body        | Body of the  | No        | [     |
|                            |             | original     |           | byte  |
|                            |             | request      |           | ]     |
+----------------------------+-------------+--------------+-----------+-------+
| SUNFISH-issuer             | header      | References   | No        | string|
|                            |             | the          |           |       |
|                            |             | application  |           |       |
|                            |             | that issued  |           |       |
|                            |             | the request. |           |       |
|                            |             | This field   |           |       |
|                            |             | may include  |           |       |
|                            |             | the data     |           |       |
|                            |             | required to  |           |       |
|                            |             | perform      |           |       |
|                            |             | application  |           |       |
|                            |             | authenticati |           |       |
|                            |             | on,          |           |       |
|                            |             | in the form  |           |       |
|                            |             | of           |           |       |
|                            |             | authenticati |           |       |
|                            |             | on           |           |       |
|                            |             | token.       |           |       |
+----------------------------+-------------+--------------+-----------+-------+
| SUNFISH-service            | header      | Machine-read | Yes       | string|
|                            |             | able         |           |       |
|                            |             | description  |           |       |
|                            |             | of endpoint  |           |       |
|                            |             | including at |           |       |
|                            |             | least an     |           |       |
|                            |             | identifier   |           |       |
|                            |             | of the       |           |       |
|                            |             | service.     |           |       |
|                            |             | With the     |           |       |
|                            |             | service id,  |           |       |
|                            |             | the PEP can  |           |       |
|                            |             | resolve      |           |       |
|                            |             | other        |           |       |
|                            |             | required     |           |       |
|                            |             | attributes.  |           |       |
+----------------------------+-------------+--------------+-----------+-------+
| SUNFISH-request            | header      | Machine-read | Yes       | string|
|                            |             | able         |           |       |
|                            |             | description  |           |       |
|                            |             | of the       |           |       |
|                            |             | target       |           |       |
|                            |             | endpoint and |           |       |
|                            |             | request      |           |       |
|                            |             | data. The    |           |       |
|                            |             | PEP at least |           |       |
|                            |             | requires the |           |       |
|                            |             | parameters   |           |       |
|                            |             | method,      |           |       |
|                            |             | port, path   |           |       |
|                            |             | and          |           |       |
|                            |             | protocol. If |           |       |
|                            |             | additional   |           |       |
|                            |             | attributes   |           |       |
|                            |             | are          |           |       |
|                            |             | registered   |           |       |
|                            |             | in the       |           |       |
|                            |             | SUNFISH      |           |       |
|                            |             | federation,  |           |       |
|                            |             | the PEP can  |           |       |
|                            |             | retrieve     |           |       |
|                            |             | these        |           |       |
|                            |             | attributes   |           |       |
|                            |             | from a       |           |       |
|                            |             | correspondin |           |       |
|                            |             |              |           |       |
|                            |             | PIP.         |           |       |
|                            |             | Furthermore, |           |       |
|                            |             | this field   |           |       |
|                            |             | may include  |           |       |
|                            |             | validity     |           |       |
|                            |             | constraints  |           |       |
|                            |             | on a request |           |       |
|                            |             | (not-valid-b |           |       |
|                            |             | efore,       |           |       |
|                            |             | not          |           |       |
|                            |             | valid-after) |           |       |
|                            |             | .            |           |       |
+----------------------------+-------------+--------------+-----------+-------+
| SUNFISH-request-parameters | header      | The          | No        | string|
|                            |             | parameters   |           |       |
|                            |             | related to   |           |       |
|                            |             | the request, |           |       |
|                            |             | including    |           |       |
|                            |             | its          |           |       |
|                            |             | priority,    |           |       |
|                            |             | SLA          |           |       |
|                            |             | requirements |           |       |
|                            |             | ,            |           |       |
|                            |             | call-back    |           |       |
|                            |             | URI. This    |           |       |
|                            |             | field        |           |       |
|                            |             | includes     |           |       |
|                            |             | other        |           |       |
|                            |             | request      |           |       |
|                            |             | meta-data    |           |       |
|                            |             | that may     |           |       |
|                            |             | extend or    |           |       |
|                            |             | override the |           |       |
|                            |             | definitions  |           |       |
|                            |             | provided in  |           |       |
|                            |             | centralized  |           |       |
|                            |             | administrati |           |       |
|                            |             | ve           |           |       |
|                            |             | console.     |           |       |
|                            |             | These        |           |       |
|                            |             | include      |           |       |
|                            |             | request      |           |       |
|                            |             | type,        |           |       |
|                            |             | application- |           |       |
|                            |             | specific     |           |       |
|                            |             | policies or  |           |       |
|                            |             | obligations  |           |       |
|                            |             | to be        |           |       |
|                            |             | applied      |           |       |
|                            |             | beyond the   |           |       |
|                            |             | ones defined |           |       |
|                            |             | in the       |           |       |
|                            |             | central      |           |       |
|                            |             | console, or  |           |       |
|                            |             | parameters   |           |       |
|                            |             | related to   |           |       |
|                            |             | data-masking |           |       |
|                            |             | policies.    |           |       |
|                            |             | The scope of |           |       |
|                            |             | applicable   |           |       |
|                            |             | and allowed  |           |       |
|                            |             | definitions  |           |       |
|                            |             | provided in  |           |       |
|                            |             | this         |           |       |
|                            |             | variable     |           |       |
|                            |             | depends on   |           |       |
|                            |             | an extent of |           |       |
|                            |             | delegation   |           |       |
|                            |             | policies, as |           |       |
|                            |             | determined   |           |       |
|                            |             | in           |           |       |
|                            |             | centralized  |           |       |
|                            |             | console.     |           |       |
+----------------------------+-------------+--------------+-----------+-------+
| SUNFISH-request-data       | header      | This field   | Yes       | string|
|                            |             | encapsulates |           |       |
|                            |             | the original |           |       |
|                            |             | header data  |           |       |
|                            |             | and the      |           |       |
|                            |             | original     |           |       |
|                            |             | query string |           |       |
|                            |             | as issued by |           |       |
|                            |             | the          |           |       |
|                            |             | application. |           |       |
+----------------------------+-------------+--------------+-----------+-------+
| SUNFISH-signature          | header      | This         | No        | string|
|                            |             | parameter is |           |       |
|                            |             | used to      |           |       |
|                            |             | ensure       |           |       |
|                            |             | integrity    |           |       |
|                            |             | and          |           |       |
|                            |             | authenticity |           |       |
|                            |             | of the       |           |       |
|                            |             | source       |           |       |
|                            |             | message for  |           |       |
|                            |             | applications |           |       |
|                            |             | which        |           |       |
|                            |             | require a    |           |       |
|                            |             | higher       |           |       |
|                            |             | degree of    |           |       |
|                            |             | security. It |           |       |
|                            |             | contains     |           |       |
|                            |             | signed       |           |       |
|                            |             | request and  |           |       |
|                            |             | fields,      |           |       |
|                            |             | according to |           |       |
|                            |             | predefined   |           |       |
|                            |             | schema       |           |       |
+----------------------------+-------------+--------------+-----------+-------+

**Responses**

+--------+-------------------------------------------------------+------------+
| Code   | Description                                           | Schema     |
+========+=======================================================+============+
| 200    | The same response as provided by the target service   | [ byte ]   |
+--------+-------------------------------------------------------+------------+
