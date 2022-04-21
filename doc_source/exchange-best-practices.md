# Best practices<a name="exchange-best-practices"></a>

Best practices for using Microsoft Exchange Server in AWS

**Topics**
+ [High availability and disaster recovery](#exchange-high-availability)
+ [Automatic failover](#exchange-failover)
+ [Security groups and firewalls](#exchange-security-groups-firewalls)

## High availability and disaster recovery<a name="exchange-high-availability"></a>

Amazon EC2 provides the ability to place instances in multiple locations composed of [Regions and Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)\. Regions are dispersed and located in separate geographic areas\. Availability Zones are distinct locations within a Region that are engineered to be isolated from failures in other Availability Zones and that provide inexpensive, low\-latency network connectivity to other Availability Zones in the same Region\.

By launching your instances in separate Regions, you can design your application to be closer to specific customers or to meet legal or other requirements\. By launching your instances in separate Availability Zones, you can protect your applications from the failure of a single location\. Exchange provides infrastructure features that complement the high availability and disaster recovery scenarios supported in the AWS Cloud\.

## Automatic failover<a name="exchange-failover"></a>

When you deploy Exchange Server with Launch Wizard, the **default parameters** configure a two\-node [database availability groups](https://docs.microsoft.com/en-us/exchange/database-availability-groups-dags-exchange-2013-help?redirectedfrom=MSDN) \(DAG\) with a file share witness\. The DAG uses Windows Server Failover Clustering for automatic failover\.

**Launch Wizard implementation supports the following scenarios:**
+  Protection from the failure of a single instance
+  Automatic failover between the cluster nodes
+ Automatic failover between Availability Zones

However, the default Launch Wizard implementation doesn’t provide automatic failover in every case\. For example, if you lost Availability Zone 1, which contains the primary node named *ExchangeNode1*, and the file share witness, this would prevent automatic failover to Availability Zone 2\. This is because the cluster would fail as it loses quorum\. In this scenario, you could follow manual disaster recovery steps that include restarting the cluster service and forcing quorum on the second cluster node *ExchangeNode2* to restore application availability\.

Launch Wizard also provides an option to deploy into three Availability Zones\. This deployment option can mitigate the loss of quorum in the case of a failure of a single node\. However, you can select this option only in AWS Regions that include three or more Availability Zones; for a current list, see [AWS Global Infrastructure](http://aws.amazon.com/about-aws/global-infrastructure/)\.

We recommend that you consult the [Microsoft Exchange Server documentation](https://docs.microsoft.com/en-us/Exchange/exchange-server?view=exchserver-2019) and customize some of the steps described in this guide, or implement steps to deploy a solution that best meets your business, IT, and security requirements\. For example, you might want a solution that deploys additional cluster nodes and configures mailbox database copies\.

## Security groups and firewalls<a name="exchange-security-groups-firewalls"></a>

AWS provides a set of building blocks, such as Amazon EC2 and Amazon VPC, that you can use to provision infrastructure for your applications\. In this model, some security capabilities, such as physical security, are the responsibility of AWS and are highlighted in the [Security Pillar](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html) of the AWS Well\-Architected Framework\. Other areas, such as controlling access to applications, fall on the application developer and the tools provided by Microsoft\.

When the EC2 instances are launched, they must be associated with a [security group](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-security-groups.html), which acts as a stateful firewall\. You have complete control over the network traffic entering or leaving the security group, and you can build granular rules that are scoped by protocol, port number, and source or destination IP address or subnet\. By default, all traffic egressing a security group is permitted\. Ingress traffic, on the other hand, must be configured to allow the appropriate traffic to reach your instances\.

The [Infrastructure Protection](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/infrastructure-protection.html) section of the Security Pillar documentation details different methods for securing your AWS infrastructure\. Recommendations include providing isolation between application tiers using security groups\. We recommend that you tightly control ingress traffic, so that you reduce the attack surface of your EC2 instances\.

Domain controllers and member servers require several security group rules to allow traffic for services such as AD DS replication, user authentication, [Windows Time service](https://docs.microsoft.com/en-us/windows-server/networking/windows-time-service/windows-time-service-top), and Distributed File System \(DFS\), among others\. The nodes running Exchange Server permit full communication between each other, as recommended by Microsoft best practices\.

Launch Wizard creates certain security groups and rules for you\. If edge node servers are configured to be deployed, they allow port 25 TCP \(SMTP\) from the entire internet\. For a detailed list of the ports allowed for Active Directory, see the [Security section](https://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-ad-best-practices.html#launch-wizard-ad-security) of the Launch Wizard for Active Directory documentation\.


**Launch Wizard for Exchange Server configures the following security groups during deployment:**  

| Security group | Associated with | Inbound source | Ports | 
| --- |--- |--- |--- |
| DomainMemberSGID | Exchange nodes, FileServer, RD Gateway, Domain controllers | VPC CIDR | Standard Active Directory ports | 
| EXCHClientSecurityGroup | Exchange nodes, FileServer | VPC CIDR | 25, 80, 443, 143, 993, 110, 995, 587 | 
| ExchangeSecurityGroup | Exchange nodes | ExchangeSecurityGroup | All ports | 
| EXCHEdgeSecurityGroup | EXCHEdgeSecurityGroup | Private subnets CIDR, 0\.0\.0\.0/0 | 50636, 25 | 
| LoadBalancerSecurityGroup | Load balancer | 0\.0\.0\.0/0 | 0\.0\.0\.0/0 | 