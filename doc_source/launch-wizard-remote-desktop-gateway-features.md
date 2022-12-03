# Features<a name="launch-wizard-remote-desktop-gateway-features"></a>

**Topics**
+ [Simple application deployment](#launch-wizard-remote-desktop-gateway-features-app-deployment)
+ [Application Resource Groups for discoverability](#launch-wizard-remote-desktop-gateway-resource-groups)
+ [AWS resource selection](#launch-wizard-remote-desktop-gateway-features-resource-selection)
+ [Cost estimation](#launch-wizard-remote-desktop-gateway-features-cost)
+ [SNS notification](#launch-wizard-remote-desktop-gateway-features-sns)
+ [Early input validation](#launch-wizard-remote-desktop-gateway-input-validation)

## Simple application deployment<a name="launch-wizard-remote-desktop-gateway-features-app-deployment"></a>

AWS Launch Wizard makes it more efficient for you to deploy third\-party applications on AWS, such as Remote Desktop Gateway\. When you input the application requirements, AWS Launch Wizard deploys the necessary AWS resources for a production\-ready application\. This means that you do not have to manage separate infrastructure pieces or spend as much time provisioning and configuring your Remote Desktop Gateway application\.

## Application Resource Groups for discoverability<a name="launch-wizard-remote-desktop-gateway-resource-groups"></a>

Launch Wizard creates a Resource Group for all of the AWS resources created for your Remote Desktop Gateway application\. You can manage the resources through the Amazon EC2 console or with AWS Systems Manager\. When you access Systems Manager through Launch Wizard, the resources are automatically filtered for you based on your Resource Group\. You can manage, patch, and maintain your Remote Desktop Gateway applications in Systems Manager\.

## AWS resource selection<a name="launch-wizard-remote-desktop-gateway-features-resource-selection"></a>

Launch Wizard considers performance, memory, bandwidth, and other application features to determine the most appropriate instance type for your Remote Desktop Gateway application\. You can modify the recommended defaults\.

## Cost estimation<a name="launch-wizard-remote-desktop-gateway-features-cost"></a>

Launch Wizard provides a cost estimate for a complete deployment\. The cost estimate is itemized for each individual resource to deploy\. The estimated cost automatically updates each time you change a resource type configuration in the wizard\. The provided estimates are for general comparisons only\. The estimates are based on On\-Demand costs and actual costs may be lower\.

## SNS notification<a name="launch-wizard-remote-desktop-gateway-features-sns"></a>

You can provide an [Amazon SNS topic](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) so that Launch Wizard will send you notifications and alerts about the status of a deployment\.

## Early input validation<a name="launch-wizard-remote-desktop-gateway-input-validation"></a>

**Launch Wizard performs the following resource limit validations at the AWS account level:**
+ VPC 
+ Internet gateway 
+ Number of AWS CloudFormation stacks