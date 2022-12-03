# How AWS Launch Wizard works<a name="how-launch-wizard-ad-works"></a>

AWS Launch Wizard provides a complete solution to provision self\-managed domain controllers on Amazon EC2 instances, or AWS Directory Service for Microsoft Active Directory, in the AWS Cloud\. You select **Microsoft Active Directory** in the wizard and provide the specifications, such as the required number of vCPUs or memory\. Based on the infrastructure requirements that you enter, Launch Wizard automatically provisions the appropriate AWS resources in the cloud\. For example, Launch Wizard recommends an appropriate instance type from the amount of vCPUs that you specify, then deploys and configures the instances\.

Launch Wizard provides an estimated cost of deployment\. You can modify your resources and instantly view an updated cost assessment\. Once you approve, Launch Wizard validates the inputs and flags inconsistencies\. After you resolve the inconsistencies, Launch Wizard provisions the resources and configures them\. The result is a ready\-to\-use Active Directory infrastructure and domain controllers\.

AWS Launch Wizard performs the following tasks to provision Active Directory domain controllers\.
+ Sets up the VPC, including private and public subnets in two Availability Zones\.\*
+ Configures two NAT gateways in the public subnets\.\*
+ Configures private and public routes\.\*
+ Enables ingress traffic into the VPC for administrative access to Remote Desktop Gateway, if specified\.
+ Uses Secrets Manager to store Domain Administrator credentials\.
+ Configures security groups and rules for traffic between instances\.
+ Sets up and configures Active Directory sites and subnets\.
+ Sets up and deploys Active Directory Certificate Services with a new Active Directory infrastructure\.
+ For existing VPCs, adds two domain controllers to an existing Active Directory infrastructure\. For the required prerequisites to add domain controllers, see [Set up for AWS Launch Wizard for Active Directory](launch-wizard-ad-setting-up.md)\.

\* If you deploy Launch Wizard into an existing VPC, the tasks in this list marked by asterisks are skipped\.

**Topics**
+ [Deployment path](launch-wizard-ad-deployment-options.md)
+ [Implementation details](launch-wizard-ad-implementation.md)