################################
Federated Security Audit (FSA)
################################


The FSA exploits machine learning techniques to figure out the high-level activities occurred in the federation and, consequently, to identify vulnerabilities and security breaches.

When vulnerabilities or security breaches are detected, the FSA raises the corresponding alert to the FRM. The FSA consists of a single module that, given in input the access control logs provided by the FRM, detects vulnerabilities and security breaches. 

Functionalities
=================

The FSA uses the following techniques:

*	*Role Mining*: the goal here is to learn roles from actual usage information and to employ the learned roles for the purpose of vulnerabilities identification.

*	*Anomaly detection*: these techniques deal with learning what is normal behaviour (profile) from historical data and comparison of profile with actual behaviour. Any significant deviation from the learned behaviour could indicate an attempt for attacks.

FSA works on a knowledge model of accesses according to the previous techniques. Specifically, it is based on three operations: 

1. *CreateModel*: it is used to learn real roles and normal users' behaviour from the log file and save the results internally. This method will be invoked iteratively (i.e. every week/month) each time with a new log file.
2. *IdentifySuspiciousActivities*: it is used to compare users' behaviour demonstrated in the input log file against learned models and alert on suspicious activities. This operation allows to check quickly (and frequently, i.e. every hour) observed users' behaviour with the learned model.
3. *GetEntitlementVulnerabilites*: is used to extract entitlement information from the learned models, and will compare existing entitlement against learned models and alert on vulnerabilities.

It is worth mentioning that the proposed operations are not exposed in terms of API. Indeed, due to the size of exchanged data, we prefer to use an approach based on file exchanges.   

Therefore, the functionalities of the FSA can be summarised as follows: 

1.	Detect vulnerabilities in existing access control mechanisms by analysing access logs.
2.	Detect security breaches by analysing access logs. 
3.	Rise alerts to the FRM to signal vulnerabilities and/or security breaches.
