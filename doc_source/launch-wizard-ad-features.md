# Features of AWS Launch Wizard<a name="launch-wizard-ad-features"></a>

**Topics**
+ [Simple application deployment](#launch-wizard-ad-features-app-deployment)
+ [AWS resource selection](#launch-wizard-ad-features-resource-selection)
+ [Cost estimation](#launch-wizard-ad-features-cost)
+ [SNS notification](#launch-wizard-ad-features-sns)
+ [Early input validation](#launch-wizard-ad-features-input-validation)
+ [Application resource groups for easy discoverability](#launch-wizard-ad-features-resource-groups)

## Simple application deployment<a name="launch-wizard-ad-features-app-deployment"></a>

AWS Launch Wizard makes it efficient for you to deploy self\-managed domain controllers and AWS Directory Service for Microsoft Active Directory on AWS\. When you enter the domain controller requirements, AWS Launch Wizard deploys the necessary AWS resources for a production\-ready environment\. This means that you do not have to manage separate infrastructure pieces or spend time provisioning and configuring your domain controllers\. 

## AWS resource selection<a name="launch-wizard-ad-features-resource-selection"></a>

Launch Wizard considers the number of Active Directory users to determine the best instance type, EBS volumes, and other resources for your domain controllers\. You can modify the recommended defaults\. 

## Cost estimation<a name="launch-wizard-ad-features-cost"></a>

Launch Wizard provides a cost estimate for the complete deployment that is itemized for each individual resource being deployed\. The estimated cost automatically updates each time you change a resource type configuration in the wizard\. However, the provided estimates are only for general comparisons\. They are based on on\-Demand costs and actual costs may be lower\.

## SNS notification<a name="launch-wizard-ad-features-sns"></a>

You can provide an [ SNS topic](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) that allows Launch Wizard to send you notifications and alerts about the status of a deployment\.

## Early input validation<a name="launch-wizard-ad-features-input-validation"></a>

You can take advantage of your existing infrastructure, such as VPC or security groups, with Launch Wizard\. This may lead to deployment failures if your existing infrastructure does not meet certain deployment prerequisites\. If these requirements are not met, the deployment will fail\. If you are in a later stage of a deployment, this failure can take more than an hour to detect\. To detect these types of issues early in the application deployment process, Launch Wizard's validation framework verifies key infrastructure specifications before provisioning\. Verification takes approximately 15 minutes\. If necessary, you can take appropriate actions to adjust your VPC configuration\. 

**Note**  
Some validations, such as for Active Directory credentials, require Application Wizard to launch a t2\.large EC2 instance in your account for a few minutes\. After it runs the necessary validations, Launch Wizard terminates the instance\.

## Application resource groups for easy discoverability<a name="launch-wizard-ad-features-resource-groups"></a>

Launch Wizard creates a resource group for all of the AWS resources created for your domain controllers\. You can manage the resources through the Amazon EC2 console or with Systems Manager\. When you access Systems Manager through Launch Wizard, the resources are automatically filtered for you based on your resource group\.