###############################
Federated Security Audit (FSA)
###############################

This is the installation for the FSA component. 

To install FSA extract the provided archive *FSA.tar.gz*. The extracted FSA folder has the following structure:

* bins/ - scripts and jars of the FSA application
* docs/ - documentation files with usage instructions 
* rootFolder/ - containing input, output and intermediate files for FSA operations: CreateModel, IdentifySuspiciousActivities and GetEntitlementVulnerabilites
* activities_arch/ - IdentifySuspiciousActivities processed input files
* activities_in/ - IdentifySuspiciousActivities input files
* activities_out/ -  IdentifySuspiciousActivities result files
* model_arch/  -  CreateModel processed input files
*	model_in/ -  CreateModel input files
*	model_out/  -  CreateModel current model	  
*	entitlement_arch/ - GetEntitlementVulnerabilites processed input files
*	entitlement_in/ - GetEntitlementVulnerabilites input files
*	entitlement_out/ - GetEntitlementVulnerabilites result files
* /spark - local installation of spark 

To use FSA application follow the following steps:

1) Start FSA application by executing bins/FSA_start.sh
2) Add input files to rootFolder/XXX_in, select XXX according to the required operation (as described above).
3) Get result files from rootFolder/XXX_out, where XXX is the same as was selected in the previous step
4) Go to step 2 or stop FSA application

