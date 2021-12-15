# What Is AWS Launch Wizard for SQL Server?<a name="what-is-launch-wizard"></a>

AWS Launch Wizard is a service that guides you through the sizing, configuration, and deployment of Microsoft SQL Server applications on AWS, following the [AWS Well\-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)\. AWS Launch Wizard supports both single instance and high availability \(HA\) application deployments\.

AWS Launch Wizard reduces the time it takes to deploy SQL Server solutions to the cloud\. You input your application requirements, including performance, number of nodes, and connectivity, on the service console\. AWS Launch Wizard identifies the right AWS resources to deploy and run your SQL Server application\. AWS Launch Wizard provides an estimated cost of deployment, and you can modify your resources and instantly view the updated cost assessment\. When you approve, AWS Launch Wizard provisions and configures the selected resources in a few hours to create a fully\-functioning production\-ready SQL Server application\. It also creates custom AWS CloudFormation templates, which can be reused and customized for subsequent deployments\.

Once deployed, your SQL Server application is ready to use and can be accessed on the EC2 console\. You can manage your SQL Server application with [AWS SSM](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html)\.

**Topics**
+ [Supported operating systems and SQL versions](#launch-wizard-os)
+ [Features of AWS Launch Wizard](launch-wizard-features.md)
+ [Components \(deployment on Windows\)](launch-wizard-components.md)
+ [Components \(deployment on Linux\)](launch-wizard-components-linux.md)
+ [Related services](related-services.md)
+ [How AWS Launch Wizard works](how-launch-wizard-works.md)
+ [Deployment options](launch-wizard-deployment-options.md)
+ [Default quotas](#launch-wizard-limits)

## Supported operating systems and SQL versions<a name="launch-wizard-os"></a>

AWS Launch Wizard supports the following operating systems and SQL Server versions:

**Deployments on Windows**
+ Windows Server 2019/2016/2012 R2
+ Enterprise and Standard Editions of Microsoft SQL Server 2019/2017/2016

**Amazon FSx for Failover Clustering \(FCI\) deployments on Windows**
+ Windows Server 2019/2016
+ Enterprise and Standard Editions of Microsoft SQL Server 2019/2017/2016 SP2

  CUs are installed at the same time as public AMIs for SQL license\-included AMIs\. CUs and service packs are not installed for license\-included Windows AMIs and BYOL AMIs\.

**Deployments on Ubuntu**
+ Ubuntu 18\.04
+ Enterprise and Standard Edition of Microsoft SQL Server 2019

**Deployments on RHEL**
+ Red Hat Enterprise Linux \(RHEL\) 7\.9
+ Enterprise and Standard Edition of Microsoft SQL Server 2019/2017

## Default quotas<a name="launch-wizard-limits"></a>

Launch Wizard allows for a maximum of 50 active applications \(with status `in progress` or `completed`\) for any given application type\. If you want to increase this limit, contact [AWS Support](http://aws.amazon.com/contact-us)\. Launch Wizard supports three parallel, in\-progress deployments per account\. 