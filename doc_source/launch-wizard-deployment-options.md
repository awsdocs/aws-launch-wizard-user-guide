# Deployment options<a name="launch-wizard-deployment-options"></a>

**Topics**
+ [Deployment on Windows](#launch-wizard-deployment-options-windows)
+ [Deployment on Linux](#launch-wizard-deployment-options-linux)

## Deployment on Windows<a name="launch-wizard-deployment-options-windows"></a>

1. **Deploy SQL Server into a new VPC across multiple Availability Zones**\. When you choose this configuration option, Launch Wizard builds a new AWS environment consisting of the VPC, subnets, NAT gateways, security groups, domain controllers, and other infrastructure components\. It then deploys Windows Server Failover Clustering \(WSFC\) with SQL Server across multiple Availability Zones into this new VPC\.

1. **Deploy SQL Server into a new VPC on a single node**\. When you choose this configuration option, Launch Wizard builds a new AWS environment consisting of the VPC, subnet, NAT gateway, security groups, domain controllers, and other infrastructure components\. It then deploys SQL Server on a single node into this new VPC\.

1. **Deploy SQL Server into an existing VPC and create a new AWS Managed Active Directory**\. When you choose this configuration option, Launch Wizard builds a new AWS environment that consists of security groups, domain controllers, and other infrastructure components, and then deploys WSFC with SQL Server in to the specified VPC and subnets\. Your AWS environment must include a VPC with two or three Availability Zones, private subnets in each Availability Zone, and at least one public subnet in the VPC\. Currently, Launch Wizard only supports AWS Managed Microsoft Active Directory for this scenario\.

1. **Deploy a SQL Server into an existing VPC with an existing AWS Managed Active Directory across multiple Availability Zones**\. When you choose this configuration option, Launch Wizard provisions WSFC across multiple Availability Zones in your existing AWS infrastructure\. Your AWS environment must include a VPC with two or three Availability Zones, private subnets in each Availability Zone, at least one public subnet in the VPC, and an AWS Active Directory in the VPC \(this is the Active Directory on which you deploy your SQL nodes\)\. 

1. **Deploy a SQL Server into an existing VPC with an existing AWS Managed Active Directory on a single node**\. When you choose this configuration option, Launch Wizard provisions SQL Server on a single node in your existing AWS infrastructure\. Your AWS environment must include a VPC in one Availability Zone, a private subnet, a public subnet in the VPC, and an AWS Active Directory in the VPC \(this is the Active Directory on which you deploy your SQL nodes\)\. 

1. **Deploy a SQL Server into an existing VPC across multiple Availability Zones and connect to an on\-premises Active Directory**\. When you choose this configuration option, Launch Wizard provisions WSFC across multiple Availability Zones in your existing AWS infrastructure\. Your AWS environment must include a VPC with two or three Availability Zones, private subnets in each Availability Zone, at least one public subnet in the VPC, and an [AWS Direct Connect](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html) connection to your on\-premises Active Directory\. 

1. **Deploy a SQL Server into an existing VPC on a single node and connect to an on\-premises Active Directory**\. When you choose this configuration option, Launch Wizard provisions SQL Server on a single node in your existing AWS infrastructure\. Your AWS environment must include a VPC in one Availability Zone, a private subnet, a public subnet in the VPC, and an [AWS Direct Connect](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html) connection to your on\-premises Active Directory\.

1. **Deploy SQL HA on Dedicated Hosts with your Windows BYOL\- or SQL Server BYOL\-licensed AMIs**\. When you choose this configuration option, Launch Wizard provisions SQL Server Always On Availability Groups on your existing Dedicated Hosts using your existing SQL Server licenses \(BYOL\) for deploying SQL Server High Availability solutions\.

Launch Wizard allows you to configure additional settings, such as the version of SQL Server \(by your choice of AMI\), in addition to instance types and Amazon EBS volume types based on the infrastructure requirements that you specify\.

## Deployment on Linux<a name="launch-wizard-deployment-options-linux"></a>

1. **Deploy SQL Server into a new VPC across multiple Availability Zones**\. When you choose this configuration option, Launch Wizard builds a new AWS environment consisting of the VPC, subnets, NAT gateways, security groups, and other infrastructure components\. It then deploys SQL Server AG across multiple Availability Zones into the VPC using Pacemaker and fencing agents for cluster management\.

1. **Deploy SQL Server into a new VPC on a single node**\. When you choose this configuration option, Launch Wizard builds a new AWS environment consisting of the VPC, subnet, NAT gateways, security groups, and other infrastructure components\. It then deploys SQL Server AG on a single node into the VPC using Pacemaker and fencing agents for cluster management\.

1. **Deploy SQL Server into an existing VPC across mulitple Availability Zones**\. When you choose this configuration option, Launch Wizard builds a new AWS environment that consists of security groups and other infrastructure components, and then deploys SQL Server AG across mulitple Availability Zones in to the specified VPC and subnets\. Your AWS environment must include a VPC with three Availability Zones, private subnets in each Availability Zone, and at least one public subnet in the VPC\. 

1. **Deploy SQL Server into an existing VPC on a single node**\. When you choose this configuration option, Launch Wizard builds a new AWS environment that consists of security groups and other infrastructure components, and then deploys SQL Server AG on a single node in to the specified VPC and subnet\. Your AWS environment must include a VPC with one Availability Zone, a private subnet, and a public subnet in the VPC\. 
