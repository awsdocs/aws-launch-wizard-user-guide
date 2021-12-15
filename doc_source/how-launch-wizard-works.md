# How AWS Launch Wizard works<a name="how-launch-wizard-works"></a>

AWS Launch Wizard provides a complete solution to provision popular third\-party applications on AWS\. Currently, Launch Wizard supports Microsoft SQL Server application deployments across multiple Availability Zones or on a single instance\. You provide the specifications, such as for performance, throughput, and networking\. Based on the application requirements that you enter, Launch Wizard automatically provisions the right AWS resources in the cloud\. For example, Launch Wizard determines the best instance type and EBS volume for your CPU, memory, and bandwidth specifications, then deploys and configures them\. 

Launch Wizard provides an estimated cost of deployment\. You can modify your resources and instantly view an updated cost assessment\. Once you approve, Launch Wizard validates the inputs and flags inconsistencies\. When the inconsistencies are resolved, Launch Wizard provisions the resources and configures them\. The result is a ready\-to\-use SQL Server Always On application\.

Launch Wizard creates a CloudFormation stack according to your infrastructure needs\. You can reuse the template created by CloudFormation as a baseline for future infrastructure provisioning\. 

For deployments on Windows, Launch Wizard supports [AWS Managed Microsoft Active Directory \(AD\)](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/directory_microsoft_ad.html)\. It also supports connecting to on\-premises Active Directory using [AWS Direct Connect](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html)\.

**Topics**
+ [Implementation details \(Windows\)](#launch-wizard-implementation)
+ [Implementation details \(Linux\)](#launch-wizard-implementation-linux)

## Implementation details for deployment on Windows<a name="launch-wizard-implementation"></a>

**Topics**
+ [SQL Server Enterprise Edition](#launch-wizard-sql)
+ [Storage on WSFC nodes](#launch-wizard-storage)
+ [IP Addressing on the Windows Server Failover Clustering \(WSFC\) nodes](#launch-wizard-ip-wsfc)
+ [Windows Server Failover Clustering \(WSFC\)](#launch-wizard-wsfc)
+ [Always On configuration](#launch-wizard-alwayson)
+ [Failover clustering](#launch-wizard-how-it-works-failover-clustering)

### SQL Server Enterprise Edition<a name="launch-wizard-sql"></a>

 Launch Wizard supports installation of SQL Server Enterprise and Standard Editions of 2016 and 2017 on Windows Server 2012 R2, 2016, and 2019 through license\-included Amazon Machine Images \(AMIs\)\. Launch Wizard allows you to bring your own SQL licenses through a custom AMI\. If you use a [custom AMI](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/Creating_EBSbacked_WinAMI.html), ensure that your AMI meets the requirements listed in [Requirements for using custom AMIs \(deployment on Windows\)](launch-wizard-setting-up.md#launch-wizard-custom-ami)\.

### Storage on WSFC nodes<a name="launch-wizard-storage"></a>

Storage capacity and performance are key aspects of any production SQL Server installation\. Launch Wizard lets you choose capacity and performance based on your deployment requirements\. 

Amazon Elastic Block Store \(Amazon EBS\) volumes are included in the architecture to provide durable, high\-performance storage\. EBS volumes are network\-attached disk storage, which you can create and attach to EC2 instances\. When attached to an instance, you can create a file system on top of the volumes, run a database, or use them in any way that you would use a block device\. EBS volumes are placed in a specific [Availability Zone](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-availability-zones), where they are automatically replicated to protect you from the failure of a single component\. EBS volume type `io1` is not supported\. 

[Provisioned IOPS EBS volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html#EBSVolumeTypes_piops) offer storage with consistent and low\-latency performance\. They are backed by solid state drives \(SSDs\) and designed for applications with I/O intensive workloads, such as databases\. Amazon EBS\-optimized instances, such as the R4 instance type, deliver dedicated throughput between Amazon EC2 and Amazon EBS\.

By default, Launch Wizard deploys three 500 GiB general purpose SSD volumes to store databases, logs, tempdb files, and backups on each WSFC node\. These general purpose SSD volumes are in addition to the root general purpose SSD volume used by the operating system, which delivers a consistent baseline of 3 IOPS/GiB and provides a total of 1,500 IOPS per volume for SQL Server database and log volumes\. You can customize the volume size and switch to using dedicated IOPS volumes with the volume you specify\. If you need more IOPS per volume, consider using Provisioned IOPS SSD volumes by changing the SQL Server Volume Type and SQL Server Volume IOPS parameters\.

 The default disk layout for SQL Server deployed by Launch Wizard is: 
+ One general purpose SSD volume \(100 GiB\) for the operating system \(C:\)
+ One general purpose SSD volume \(500 GiB\) to host the SQL Server database files \(D:\)
+ One general purpose SSD volume \(500 GiB\) to host the SQL Server log files \(E:\)
+ One general purpose SSD volume \(500 GiB\) to host the SQL Server tempdb and backup files \(F:\)

### IP Addressing on the Windows Server Failover Clustering \(WSFC\) nodes<a name="launch-wizard-ip-wsfc"></a>

In order to support WSFC and Always On Availability Group listeners, each node that hosts the SQL Server instances that participate in the cluster must have three IP addresses assigned, as follows:
+ One IP address as the primary IP address for the instance
+ A second IP address as the WSFC IP resource
+ A third IP address to host the Always On Availability Group listener

When you launch the AWS CloudFormation template, you can specify the addresses for each node\. By default, the underlying CloudFormation templates of Launch Wizard use 10\.0\.0\.0/20, 10\.0\.16\.0/20, and 10\.0\.32\.0/20 as CIDR blocks for the private subnets\. This is true only when you use Launch Wizard to deploy SQL Server Always On clusters in a new VPC\.

### Windows Server Failover Clustering \(WSFC\)<a name="launch-wizard-wsfc"></a>

You can build the failover cluster after your Windows Server instances have been deployed and domain\-joined\. Launch Wizard's underlying AWS CloudFormation templates build the cluster when deploying the second node\. If you use the default template parameter settings, Launch Wizard executes the following Windows PowerShell commands to complete this task\. 

```
PS C:\> Install-WindowsFeature failover-clustering –IncludeManagementTools
```

```
PS C:\> New-Cluster –Name WSFClusterName –Node $nodes -StaticAddress $addr
```

The first command runs on each instance during the bootstrapping process\. It installs the required components and management tools for the failover clustering services\. The second command runs near the end of the bootstrapping process on the second node and is responsible for creating the cluster and for defining the server nodes and IP addresses\.

If you set the optional third Availability Zone, Launch Wizard keeps the quorum settings to the default node majority\.

```
PS C:\> Set-ClusterQuorum –NodeMajority
```

### Always On configuration<a name="launch-wizard-alwayson"></a>

After SQL Server Enterprise edition has been installed and the Windows Server failover cluster has been built, Launch Wizard enables SQL Server Always On with the following PowerShell command\.

```
PS C:\> Enable-SqlAlwaysOn –ServerInstance $ServerInstance
```

Launch Wizard runs this command on each node, and the proper server name is provided as a value for the `ServerInstance` parameter\.

When the deployment is complete, Launch Wizard creates your databases and make them highly available by creating an Always On Availability Group\.

When you create an availability group, you provide a network share that is used to perform an initial data synchronization\. As you progress through the New Availability Group Wizard, a full backup for each selected database is created and placed in the share\. The secondary node connects to the share and restores the database backups before joining the availability group\.

### Failover clustering<a name="launch-wizard-how-it-works-failover-clustering"></a>

Launch Wizard sets up and installs SQL Server failover clustering on Windows on each of the nodes that uses PowerShell DSC modules, which is the first step it takes to configure the cluster\. Next, Launch Wizard adds the specified account as the domain account to the cluster group\. 

You can use either license\-included AMIs or your own custom AMIs\. If you use license\-included AMIs, they must be Windows or SQL license\-included AMIs\. 

## Implementation details for deployment on Linux<a name="launch-wizard-implementation-linux"></a>

AWS Launch Wizard implements SQL Server deployments on Linux as follows\.

### Always On availability group configuration<a name="launch-wizard-alwayson"></a>
+ **Availability group enablement**\. Launch Wizard enables availability groups using the following commands\.

  ```
  sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled 1
  ```

  ```
  sudo systemctl restart mssql-server
  ```
+ **Authentication of TCP endpoints**\. An availability group uses TCP endpoints for communication\. Launch Wizard uses certificate\-based authentication to support endpoints for availability groups\. All nodes create a self\-signed certificate and upload their certificates to an Amazon S3 location that you specify\. Each node will then locally sync the certificates from all of the other nodes\.
+ **Creation of availability group endpoints**\. Launch Wizard performs the backup, create, and restore of the authentication certificates with Transact\-SQL on Linux\. Transact\-SQL on Linux is also used to create the availability group endpoints and availability groups with automatic seeding\. The listener and read\-only routing are configured after an availability group is created\. 
+ ****Creation of availability group resources in Pacemaker cluster****\. After an availability group is created in SQL Server, the corresponding resources must be created in Pacemaker\. There are two resources associated with an availability group: the availability group and a floating IP address\. The created availability group resource is a unique resource called a clone\. The availability group resource contains copies on each node, with one controlling resource\. The controlling resource is associated with the server that hosts the primary replicas\. The secondary replicas \(except for configuration\-only replicas\) can be promoted to controlling resource during failover\. 

  The floating IP address is an unused IP address in the VPC that is not part of any of the subnet CIDR ranges that are part of this cluster\. Launch Wizard creates a DNS entry in the host files to map AGListenerName to a floating IP Address\. Launch Wizard also creates a route for Floating IP/32 to the Primary Node Network Interface so that all internal VPC network traffic is routed to the primary node when any traffic reaches the floating IP\. In case of failover, the resource agent will update the routing table to point the floating IP to the new primary node\.
+ **Creation of availability group listener**\. The creation of the availability group listener is performed using the following Transact\-SQL commands:

  ```
  ALTER AVAILABILITY GROUP MyAg2
  ```

  ```
  ADD LISTENER ...
  ```