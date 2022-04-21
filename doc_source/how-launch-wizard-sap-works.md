# How AWS Launch Wizard for SAP works<a name="how-launch-wizard-sap-works"></a>

AWS Launch Wizard provisions and configures the infrastructure required to run SAP HANA database\- and NetWeaver\-based SAP applications on AWS\. You select the SAP deployment pattern and provide the specifications, such as operating system, instance size, and vCPU/memory\. Or, Launch Wizard can make these selections for you according to [SAP Standard Application Benchmarks \(SAPS\)](https://www.sap.com/about/benchmark/measuring.html)\. You have the option to manually choose the instance\. Based on your selections, Launch Wizard automatically provisions the necessary AWS resources in the cloud\. 

Launch Wizard recommends Amazon EC2 instances by evaluating the [SAPS](https://www.sap.com/about/benchmark/measuring.html) or vCPU/memory requirements against the performance of Amazon EC2 instances supported by AWS\. When new EC2 instances are released and certified for SAP, the sizing feature of Launch Wizard will take them into consideration when proposing recommendations\.

Launch Wizard maintains a mapping rule engine built on the list of certified EC2 instances that are supported by SAP\. When you enter your vCPU/memory or SAPS requirements, Launch Wizard recommends an Amazon EC2 instance that is certified for SAP workloads and offers performance that is no less than your input requirements\. For certain workloads, such as SAP HANA in a production environment, Launch Wizard recommends instances based on the official SAP recommendations for SAP HANA database workloads\. For workloads in a non\-production environment, Launch Wizard recommends Amazon EC2 instances that meet SAP recommended requirements; however, the recommended instances are not enforced\. You can change the instance types after deployment, or you can override the recommendation by making manual selections\. 

In addition to launching instances based on the SAP system information that you provide, such as SAP System Number and SAP System Identifier \(SAP SID\), Launch Wizard performs the following operations:
+ Configures the operating system
+ Configures hostname
+ Attaches security groups so that the systems in the cluster that use the same configuration template, and also external systems, can communicate with the SAP systems that will be deployed on these instances\.

Launch Wizard provides an estimated cost of deployment\. You can modify your resources and instantly view an updated cost assessment\. After you approve the deployment, Launch Wizard validates the inputs and flags inconsistencies\. After you resolve the inconsistencies, Launch Wizard provisions and configures the resources\. The result is a ready\-to\-use SAP application\.

Launch Wizard creates a CloudFormation stack according to your infrastructure needs\. For more information, see [Working With Stacks](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacks.html) in the *AWS CloudFormation User Guide*\.

**Topics**
+ [Implementation Details](launch-wizard-sap-implementation.md)