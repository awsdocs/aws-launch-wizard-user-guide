# Components<a name="launch-wizard-exchange-components"></a>

An Exchange environment deployed with Launch Wizard will include the following components:
+  A highly available architecture that spans two or three Availability Zones\. 
+  An Amazon VPC configured with public and private subnets, according to AWS best practices, to provide you with your own virtual network on AWS\. 
+  In the public subnets:
  +  Managed network address translation \(NAT\) gateways to allow outbound internet access for resources in the private subnets\.
  +  \(Optional\) A Remote Desktop Gateway in an Auto Scaling group to allow inbound Remote Desktop Protocol \(RDP\) access to Amazon EC2 instances in public and private subnets\.
  + \(Optional\) Exchange Edge Transport servers for routing internet email in and out of your environment\.
+  In the private subnets: 
  + Active Directory domain controllers\.
  + Windows Server EC2 instances functioning as Exchange nodes\.

By default, Launch Wizard deploys Exchange using two Availability Zones\. You can also choose to use three Availability Zones which enable automatic failover of [database availability groups](https://docs.microsoft.com/en-us/exchange/database-availability-groups-dags-exchange-2013-help?redirectedfrom=MSDN) \(DAGs\)\. When using a third Availability Zone, you can specify whether to deploy a full Exchange node or a file share witness\. For more information about automatic failover for the DAGs, see [Configure and manage quorum](https://docs.microsoft.com/en-us/windows-server/failover-clustering/manage-cluster-quorum) in the Microsoft documentation\.

You can choose to use an internal Application Load Balancer as part of the deployment to provide high availability and distribute traffic to the Exchange nodes\. In this configuration, you need to import a Secure Sockets Layer \(SSL\) certificate into AWS Certificate Manager before deploying Exchange with Launch Wizard\.

AWS Secrets Manager is used to securely store the Exchange administrative account credentials\. SSM Parameter Store is used to retrieve the credentials when necessary\.

You can build your Exchange environment with two Availability Zones as shown in the following diagram\.

![\[An image of Exchange server deployed in two Availability Zones.\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/exchange-on-aws-architecture-2az.png)

You can also build your Exchange environment with three Availability Zones to provide automatic failover of the DAGs as shown in the following diagram\.

![\[An image of Exchange server deployed in three Availability Zones.\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/exchange-on-aws-architecture-3az.png)