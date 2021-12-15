# Features of AWS Launch Wizard<a name="launch-wizard-features"></a>

**Topics**
+ [Simple application deployment](#launch-wizard-features-app-deployment)
+ [AWS resource selection](#launch-wizard-features-resource-selection)
+ [Cost estimation](#launch-wizard-features-cost)
+ [Reusable code templates](#launch-wizard-features-code-templates)
+ [SNS notification](#launch-wizard-features-sns)
+ [Always On Availability Groups \(SQL Server\)](#launch-wizard-features-allways-on)
+ [Dedicated Hosts \(deployment on Windows\)](#launch-wizard-features-dedicated-hosts)
+ [Early input validation](#launch-wizard-features-input-validation)
+ [Application resource groups for easy discoverability](#launch-wizard-features-resource-groups)
+ [One\-click monitoring](#launch-wizard-features-application-insights)
+ [Amazon FSx for Failover Clustering \(FCI\)](#launch-wizard-features-fci)

## Simple application deployment<a name="launch-wizard-features-app-deployment"></a>

AWS Launch Wizard makes it easy for you to deploy third\-party applications on AWS, such as Microsoft SQL Server\. When you input the application requirements, AWS Launch Wizard deploys the necessary AWS resources for a production\-ready application\. This means that you do not have to manage separate infrastructure pieces or spend time provisioning and configuring your SQL Server application\. 

## AWS resource selection<a name="launch-wizard-features-resource-selection"></a>

Launch Wizard considers performance, memory, bandwidth, and other application features to determine the best instance type, EBS volumes, and other resources for your SQL Server application\. You can modify the recommended defaults\. 

## Cost estimation<a name="launch-wizard-features-cost"></a>

Launch Wizard provides a cost estimate for a complete deployment\. The cost estimate is itemized for each individual resource to deploy\. The estimated cost automatically updates each time you change a resource type configuration in the wizard\. The provided estimates are for general comparisons only\. The estimates are based on On\-Demand costs and actual costs may be lower\.

## Reusable code templates<a name="launch-wizard-features-code-templates"></a>

Launch Wizard creates a CloudFormation stack that can be reused to customize and replicate your infrastructure in multiple environments\. Code in the template helps you provision resources\. You can access and use the templates created by your Launch Wizard deployment from the CloudFormation console\. For more information about CloudFormation stacks, see [Working with stacks](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacks.html)\.

## SNS notification<a name="launch-wizard-features-sns"></a>

You can provide an [SNS topic](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) so that Launch Wizard will send you notifications and alerts about the status of a deployment\.

## Always On Availability Groups \(SQL Server\)<a name="launch-wizard-features-allways-on"></a>

Always On Availability Groups \(AG\) is a Microsoft SQL Server feature that is supported by the AWS SQL Server installation\. AG augments the availability of a set of user databases\. An availability group supports a failover environment for a discrete set of user databases, known as availability databases\. If one of these databases fails, another database takes over its workload with no impact on availability\. Always On Availability improves database availability, enabling more efficient resource usage\. For more information about the concepts and benefits of Always On Availability, see [ Always On Availability Groups \(SQL Server\)](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-2017)\.

## Dedicated Hosts \(deployment on Windows\)<a name="launch-wizard-features-dedicated-hosts"></a>

You can deploy SQL Server Always On Availability Groups \(AG\) or basic availability groups on your Dedicated Hosts to leverage your existing SQL Server Licenses \(BYOL\)\. From the Launch Wizard console, select **Dedicated Host** tenancy, and then select the Dedicated Hosts for your VPC\. For more information about Amazon EC2 Dedicated Hosts, see [Dedicated Hosts](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/dedicated-hosts-overview.html)\.

## Early input validation<a name="launch-wizard-features-input-validation"></a>

You can leverage your existing infrastructure \(such as VPC or Active Directory\) with Launch Wizard\. This may lead to deployment failures if your existing infrastructure does not meet certain deployment prerequisites\. For example, for a SQL Server Always On deployment in your existing VPC, the VPC must have at least one public subnet and two private subnets\. It must also have outbound connectivity to Amazon S3, Systems Manager, and AWS CloudFormation service endpoints\. If these requirements are not met, the deployment will fail\. If you are in a later stage of a deployment, this failure can take more than an hour to detect\. To detect these types of issues early in the application deployment process, Launch Wizard's validation framework verifies key application and infrastructure specifications before provisioning\. Verification takes approximately 15 minutes\. If necessary, you can take appropriate actions to adjust your VPC configuration\. 

Launch Wizard performs the following infrastructure validations:

**Resource limit validations at the AWS account level:**
+ VPC 
+ Internet gateway 
+ Number of CloudFormation stacks

**Additionally, Launch Wizard performs the following application\-specific validations:**
+ Active Directory credentials \(deployment on Windows\)
+ Public subnet outbound connectivity
+ Private subnet outbound connectivity
+ Custom Windows AMIs:
  + SQL Server installed and running on instance
  + Compliant versions of Windows and SQL Server
+ Dedicated Hosts \(deployment on Windows\)
  + AMIs are filtered according to the billing code\. When you select Dedicated Host tenancy in the application, the AMI selection dropdown list filters out AMIs for which the usage operation is set to include SQL Server Enterprise or SQL Server Standard, per the [ details and usage operation values](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ami-billing-info.html#billing-info)\. This filtering behavior is the result of restrictions described in the [ Dedicated Host restrictions](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-hosts-overview.html#dedicated-hosts-limitations) page\. 
  + Supported instance type
  + Sufficient capacity to launch number of nodes and instances
  + Selected subnet and corresponding Dedicated Host are in the same Availability Zone for any additional nodes beyond the primary and first secondary nodes

**Note**  
Some validations, for example for valid Active Directory credentials, require Application Wizard to launch a `t2.large` EC2 instance in your account for a few minutes\. After it runs the necessary validations, Launch Wizard terminates the instance\.

## Application resource groups for easy discoverability<a name="launch-wizard-features-resource-groups"></a>

Launch Wizard creates a resource group for all of the AWS resources created for your SQL Server application\. You can manage the resources through the EC2 console or with Systems Manager\. When you access Systems Manager through Launch Wizard, the resources are automatically filtered for you based on your resource group\. You can manage, patch, and maintain your SQL Server applications in Systems Manager\.

## One\-click monitoring<a name="launch-wizard-features-application-insights"></a>

Launch Wizard integrates with [CloudWatch Application Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch-application-insights.html) to provide a one\-click monitoring setup experience for deploying SQL Server HA workloads on AWS\. When you select the option to set up monitoring and insights with Application Insights on the Launch Wizard console, Application Insights automatically sets up relevant metrics, logs, and alarms on CloudWatch, and starts monitoring newly deployed workloads\. You can view automated insights and detected problems, along with the health of your SQL Server HA workloads, on the CloudWatch console\.

Counters that you can configure using Application Insights include:
+ Mirrored Write Transaction/sec
+ Recovery Queue Length
+ Transaction delay
+ Windows Event Logs on CloudWatch

You can also get automated insights when a failover event or problem, such as a restricted access to query a target database, is detected on your workload\.

## Amazon FSx for Failover Clustering \(FCI\)<a name="launch-wizard-features-fci"></a>

Launch Wizard uses Amazon FSx to provide Failover Clustering for SQL Server deployments\. Failover Clustering is a high availability solution for SQL that puts all database and log files in shared storage \(Amazon FSx\)\. The Amazon FSx file share spans multiple Availability Zones and is highly redundant, allowing for automatic failover between SQL nodes in the event of failure\.