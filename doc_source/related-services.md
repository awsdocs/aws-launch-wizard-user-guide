# Related services<a name="related-services"></a>

**Topics**
+ [AWS CloudFormation](#launch-wizard-related-services-cloudformation)
+ [AWS SSM](#launch-wizard-related-services-ssm)
+ [Amazon Simple Notification Service \(SNS\)](#launch-wizard-related-services-sns)
+ [Amazon CloudWatch Application Insights](#launch-wizard-related-services-application-insights)
+ [Linux\-only technologies](#launch-wizard-related-services-linux)

## AWS CloudFormation<a name="launch-wizard-related-services-cloudformation"></a>

[AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) is a service for modeling and setting up your AWS resources, enabling you to spend more time focusing on your applications that run in AWS\. You create a template that describes all of the AWS resources that you want to use \(for example, Amazon EC2 instances or Amazon RDS DB instances\), and AWS CloudFormation takes care of provisioning and configuring those resources for you\. With Launch Wizard, you don’t have to sift through CloudFormation templates to deploy your application\. Instead, Launch Wizard combines infrastructure provisioning and configuration \(with a CloudFormation template\) and application configuration \(with code that runs on EC2 instances to configure the application\) into a unified SSM Automation document\. The SSM document is then invoked by Launch Wizard’s backend service to provision a SQL Server application in your account\. For more information, see the * [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/)*\.

## AWS SSM<a name="launch-wizard-related-services-ssm"></a>

[AWS SSM](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html) is a collection of capabilities for configuring and managing your Amazon EC2 instances, on\-premises servers and virtual machines, and other AWS resources at scale\. Systems Manager includes a unified interface that enables you to centralize operational data and automate tasks across your AWS resources\. Systems Manager shortens the time to detect and resolve operational problems in your infrastructure\. You have the option of managing your application with Systems Manager after deploying with Launch Wizard\. For more information, see the *[AWS Systems Manager User Guide](https://docs.aws.amazon.com/systems-manager/latest/userguide/)*\.

## Amazon Simple Notification Service \(SNS\)<a name="launch-wizard-related-services-sns"></a>

[Amazon Simple Notification Service \(SNS\)](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) is a highly available, durable, secure, fully managed pub/sub messaging service that provides topics for high\-throughput, push\-based, many\-to\-many messaging\. Using Amazon SNS topics, your publisher systems can fan out messages to a large number of subscriber endpoints and send notifications to end users using mobile push, SMS, and email\. You can use SNS topics for your Launch Wizard deployments to stay up\-to\-date on deployment progress\. For more information, see the [https://docs.aws.amazon.com/sns/latest/dg/welcome.html](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\.

## Amazon CloudWatch Application Insights<a name="launch-wizard-related-services-application-insights"></a>

[Amazon CloudWatch Application Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch-application-insights.html) facilitates observability for \.NET and SQL Server applications\. It can help you set up the best monitors for your application resources to continuously analyze data for signs of problems with your applications\. Application Insights, which is powered by [Sagemaker](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html) and other AWS technologies, provides automated dashboards that show potential problems with monitored applications, helping you to quickly isolate ongoing issues with your applications and infrastructure\. The enhanced visibility into the health of your applications that Application Insights provides can help you reduce your mean time to repair \(MTTR\) so that you don't have to pull in multiple teams and experts to troubleshoot your application issues\.

## Linux\-only technologies<a name="launch-wizard-related-services-linux"></a>

The following key technologies are used when you deploy a SQL Server application with Amazon Launch Wizard to the Linux platform\.
+ **[Pacemaker](http://manpages.ubuntu.com/manpages/bionic/man8/crm_node.8.html)** is an open source cluster resource manager \(CRM\), which is a system that coordinates managed resources and services made highly available by a cluster\.
+ **[Corosync](https://corosync.github.io/corosync/)** is an open source program that provides cluster membership and messaging capabilities, often referred to as the messaging layer, to client servers\. In contrast to Pacemaker, which allows you to control cluster behavior, Corosync makes it possible for servers to communicate as a cluster\.
+ **[Transact\-SQL](https://docs.microsoft.com/en-us/sql/t-sql/language-reference?view=sql-server-ver15)** is an extension to the SQL language\. It is used to interact with relational databases\. Transact\-SQL is platform\-agnostic and can be used to configure the AlwaysOn Availability Group and listener\.
+ **[Fencing](https://ubuntu.com/server/docs/ubuntu-ha-introduction)** is used to isolate a malfunctioning server from the cluster in order to protect and secure the synced resources\. The recommended solution to use in the case of a malfucntioning server is the "Shoot the other node in the head" \(STONITH\) method\. STONITH is a fencing technique that isolates a failed node so that it does not disrupt a computer cluster\. The STONITH method fences failed nodes by resetting or powering down the failed node\. Fencing is also used when a clustered service cannot be stopped\. In this case, the cluster uses fencing to force the whole node offline, which makes it safe to start the service from a different server\. Fencing can be performed at two levels: the node or resource level\. Launch Wizard only supports node\-level fencing\. 