# Set up<a name="launch-wizard-remote-desktop-gateway-set-up"></a>

**Topics**
+ [Specialized knowledge](#launch-wizard-remote-desktop-gateway-specialized-knowledge)
+ [Amazon Web Services account](#launch-wizard-remote-desktop-gateway-aws-account)
+ [Service Quotas](#launch-wizard-remote-desktop-gateway-resource-quotas)
+ [Amazon Elastic Compute Cloud key pairs](#launch-wizard-remote-desktop-gateway-key-pairs)
+ [AWS Identity and Access Management permissions](#launch-wizard-remote-desktop-gateway-iam-permissions)

## Specialized knowledge<a name="launch-wizard-remote-desktop-gateway-specialized-knowledge"></a>

This deployment requires a moderate level of familiarity with AWS services\. If you’re new to AWS, see [Getting Started Resource Center](http://aws.amazon.com/getting-started) and [AWS Training and Certification](http://aws.amazon.com/training)\. These sites provide materials for learning how to design, deploy, and operate your infrastructure and applications on the AWS Cloud\. 

This Launch Wizard assumes familiarity with Remote Desktop Gateway\.

## Amazon Web Services account<a name="launch-wizard-remote-desktop-gateway-aws-account"></a>

If you don’t already have an AWS account, create one at [https://aws\.amazon\.com]() by following the on\-screen instructions\. Part of the sign\-up process involves receiving a phone call and entering a PIN using the phone keypad\.

Your AWS account is automatically signed up for all AWS services\. You are charged only for the services you use\. 

## Service Quotas<a name="launch-wizard-remote-desktop-gateway-resource-quotas"></a>

If necessary, [request service quota increases](https://console.aws.amazon.com/servicequotas/) for the following resources\. You might need to request increases if your existing deployment currently uses these resources and if this Launch Wizard deployment could result in exceeding the default quotas\. The [Service Quotas console](https://console.aws.amazon.com/servicequotas/) displays your usage and quotas for some aspects of some services\. For more information, see [What is Service Quotas?](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html) and [AWS service quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\.

Existing VPC Service Quotas:


| Resource | Default quota | This deployment uses | 
| --- | --- | --- | 
|  Elastic IP Addresses  | 5 per Region |  2  | 
| AWS Identity and Access Management \(IAM\) security groups | 300 per account |  1  | 
|  IAM roles  | 1,000 per account |  1  | 
|  Auto Scaling groups  | 200 per Region |  1  | 
| Amazon EC2 On\-Demand Instances \(Standard\) | 5 per Region | 1\-4 | 

New VPC Service Quotas:


| Resource | Default quota | This deployment uses | 
| --- | --- | --- | 
| VPCs | 5 per Region | 1 | 
|  Elastic IP Addresses  | 5 per Region |  2  | 
| Internet Gateway | 5 per Region | 1 | 
| AWS Identity and Access Management \(IAM\) security groups | 300 per account |  1  | 
|  IAM roles  | 1,000 per account |  1  | 
|  Auto Scaling groups  | 200 per Region |  1  | 
| Amazon EC2 On\-Demand Instances \(Standard\) | 5 per Region | 1\-4 | 

## Amazon Elastic Compute Cloud key pairs<a name="launch-wizard-remote-desktop-gateway-key-pairs"></a>

Ensure that at least one Amazon EC2 key pair exists in your AWS account in the Region where you plan to deploy the Launch Wizard application\. Note the key pair name because you will use it during deployment\. To create a key pair, see [Amazon EC2 key pairs and Windows instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-key-pairs.html)\.

For testing or proof\-of\-concept purposes, we recommend creating a new key pair instead of using one that’s already being used by a production instance\. 

## AWS Identity and Access Management permissions<a name="launch-wizard-remote-desktop-gateway-iam-permissions"></a>

Before deploying the Launch Wizard application, you must sign in to the AWS Management Console with IAM permissions for the resources that the templates deploy\. The *AdministratorAccess* managed policy within IAM provides sufficient permissions, although your organization may choose to use a custom policy with more restrictions\. For more information, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html)\.