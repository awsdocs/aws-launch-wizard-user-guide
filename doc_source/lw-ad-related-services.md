# Related services<a name="lw-ad-related-services"></a>

**Topics**
+ [AWS CloudFormation](#launch-wizard-ad-related-services-cloudformation)
+ [Amazon Simple Notification Service \(SNS\)](#launch-wizard-ad-related-services-sns)
+ [Amazon CloudWatch Logs](#launch-wizard-ad-related-services-cloudwatch-logs)
+ [AWS Secrets Manager](#launch-wizard-ad-related-services-secrets-manager)

## AWS CloudFormation<a name="launch-wizard-ad-related-services-cloudformation"></a>

[AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) is a service for modeling and setting up your AWS resources, enabling you to spend more time focusing on your applications that run in AWS \. You create a template that describes all of the AWS resources that you want to use \(for example, EC2 instances\), and AWS CloudFormation provisions and configures those resources for you\. With Launch Wizard, you don’t have to sift through CloudFormation templates to deploy your application\. Instead, Launch Wizard combines infrastructure provisioning and configuration \(with an AWS CloudFormation template and PowerShell scripts\) to provision a new Active Directory infrastructure or additional domain controllers in your account\. For more information, see the * [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/)*\.

## Amazon Simple Notification Service \(SNS\)<a name="launch-wizard-ad-related-services-sns"></a>

[Amazon Simple Notification Service](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) \(Amazon SNS\) is a highly available, durable, secure, fully managed publish/subscribe messaging service that provides topics for high\-throughput, push\-based, many\-to\-many messaging\. Using Amazon SNS topics, your publisher systems can fan out messages to a large number of subscriber endpoints and send notifications to end users using mobile push, SMS, and email\. You can use Amazon SNS topics for your Launch Wizard deployments to stay up to date on deployment progress\. For more information, see the [https://docs.aws.amazon.com/sns/latest/dg/welcome.html](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\.

## Amazon CloudWatch Logs<a name="launch-wizard-ad-related-services-cloudwatch-logs"></a>

[Amazon CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) enables you to centralize the logs from all of your systems, applications, and AWS services that you use, in a single, highly scalable service\. You can then easily view them, search them for specific error codes or patterns, filter them based on specific fields, or archive them securely for future analysis\. Amazon CloudWatch Logs enables you to see all of your logs, regardless of their source, as a single and consistent flow of events ordered by time, and you can query them and sort them based on other dimensions, group them by specific fields, create custom computations with a powerful query language, and visualize log data in dashboards\. Launch Wizard streams provisioning logs from all of the AWS log sources that you can view on the CloudWatch console\.

## AWS Secrets Manager<a name="launch-wizard-ad-related-services-secrets-manager"></a>

With [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/) you can replace hard\-coded credentials in your code, including passwords, with an API call to Secrets Manager to programmatically retrieve the secret\. This helps ensure the secret can't be compromised by someone examining your code\. Also, you can configure Secrets Manager to automatically rotate the secret for you according to a specified schedule\. Launch Wizard uses Secrets Manager to join your domain controllers to Active Directory and promote them\.