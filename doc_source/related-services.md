# Related Services<a name="related-services"></a>

**Topics**
+ [AWS CloudFormation](#launch-wizard-related-services-cloudformation)
+ [AWS Systems Manager](#launch-wizard-related-services-ssm)
+ [Amazon Simple Notification Service \(SNS\)](#launch-wizard-related-services-sns)

## AWS CloudFormation<a name="launch-wizard-related-services-cloudformation"></a>

[AWS CloudFormation ](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) is a service for modeling and setting up your AWS resources, enabling you to spend more time focusing on your applications that run in AWS\. You create a template that describes all of the AWS resources that you want to use \(for example, Amazon EC2 instances or Amazon RDS DB instances\), and AWS CloudFormation takes care of provisioning and configuring those resources for you\. With Launch Wizard, you don’t have to sift through CloudFormation templates to deploy your application\. Instead, Launch Wizard combines infrastructure provisioning and configuration \(with a CloudFormation template\) and application configuration \(with code that runs on EC2 instances to configure the application\) into a unified SSM Automation document\. The SSM document is then invoked by Launch Wizard’s backend service to provision a SQL Server Always On application in your account\. For more information, see the * [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/)*\.

## AWS Systems Manager<a name="launch-wizard-related-services-ssm"></a>

[AWS Systems Manager ](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html) is a collection of capabilities for configuring and managing your Amazon EC2 instances, on\-premises servers and virtual machines, and other AWS resources at scale\. Systems Manager includes a unified interface that enables you to centralize operational data and automate tasks across your AWS resources\. Systems Manager shortens the time to detect and resolve operational problems in your infrastructure\. You have the option of managing your application with Systems Manager after deploying with Launch Wizard\. For more information, see the *[AWS Systems Manager User Guide](https://docs.aws.amazon.com/systems-manager/latest/userguide/)*\.

## Amazon Simple Notification Service \(SNS\)<a name="launch-wizard-related-services-sns"></a>

[Amazon Simple Notification Service \(SNS\)](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) is a highly available, durable, secure, fully managed pub/sub messaging service that provides topics for high\-throughput, push\-based, many\-to\-many messaging\. Using Amazon SNS topics, your publisher systems can fan out messages to a large number of subscriber endpoints and send notifications to end users using mobile push, SMS, and email\. You can use SNS topics for your Launch Wizard deployments to stay up\-to\-date on deployment progress\. For more information, see the [https://docs.aws.amazon.com/sns/latest/dg/welcome.html](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\.
