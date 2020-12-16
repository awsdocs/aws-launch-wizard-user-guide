# How AWS Launch Wizard works<a name="how-launch-wizard-ad-works"></a>

AWS Launch Wizard provides a complete solution to provision self\-managed domain controllers on AWS\. You select **Active Directory** in the wizard and provide the specifications, such as number of users\. Based on the infrastructure requirements that you enter, Launch Wizard automatically provisions the right AWS resources in the cloud\. For example, Launch Wizard determines the best instance type and EBS volume for your number of users, then deploys and configures them\. 

Launch Wizard provides an estimated cost of deployment\. You can modify your resources and instantly view an updated cost assessment\. Once you approve, Launch Wizard validates the inputs and flags inconsistencies\. After you resolve the inconsistencies, Launch Wizard provisions the resources and configures them\. The result is a ready\-to\-use Active Directory infrastructure and domain controllers\.

AWS Launch Wizard performs the following tasks to provision self\-managed domain controllers\.
+ Sets up the VPC, including private and public subnets in two Availability Zones\.\*
+ Configures two NAT gateways in the public subnets\.\*
+ Configures private and public routes\.\*
+ Enables ingress traffic into the VPC for administrative access to Remote Desktop Gateway\.
+ Stores the trust administrator password in Secrets Manager \.
+ Uses Secrets Manager to generate Domain Administrator passwords\.
+ Launches instances using the specified version of Windows Server\.
+ Configures security groups and rules for traffic between instances\.
+ Sets up and configures Active Directory sites and subnets\.
+ For existing VPCs, optionally sets up forest trusts with other Active Directory forests\. For the required prerequisites to set up forest trusts, see [Trust relationships](launch-wizard-ad-setting-up.md#launch-wizard-ad-setup-trusts)\. For information about creating forest trusts, see [Configure forest trust relationships](launch-wizard-ad-create-trusts.md)\.
+ Sets up and deploys Active Directory Certificate Services with a new Active Directory infrastructure\.
+ For existing VPCs, adds up to three domain controllers to an existing Active Directory infrastructure\. For the required prerequisites to add domain controllers, see [Set up for AWS Launch Wizard for Active Directory](launch-wizard-ad-setting-up.md)\.

\* If you deploy Launch Wizard into an existing VPC, the tasks in this list marked by asterisks are skipped\.

**Topics**
+ [Deployment path](launch-wizard-ad-deployment-options.md)
+ [Implementation details](launch-wizard-ad-implementation.md)
