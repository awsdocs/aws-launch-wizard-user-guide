# Implementation details<a name="launch-wizard-ad-implementation"></a>

This section describes how Launch Wizard implements an Active Directory Domain Services \(AD DS\) deployment in the AWS Cloud\. It includes details about how to use Amazon VPC to define your networks in the cloud, and information about domain controller placement, Active Directory Sites and Services configuration, and how DNS and DHCP work in an Amazon Virtual Private Cloud \(Amazon VPC\)\.

**Topics**
+ [VPC](#launch-wizard-ad-implementation-vpc)
+ [Security groups](#launch-wizard-ad-implementation-sg)
+ [Remote Desktop Gateway](#launch-wizard-ad-implementation-rdg)
+ [Active Directory](#launch-wizard-ad-implementation-ad)
+ [Self\-managed domain controller architecture](#launch-wizard-ad-architecture)

## VPC<a name="launch-wizard-ad-implementation-vpc"></a>

You can define a virtual network topology that closely resembles a traditional on\-premises network using Amazon VPC\. A VPC can span multiple Availability Zones place independent infrastructure in physically separate locations\. A multi\-Availability Zone deployment results in high availability and fault tolerance\. Launch Wizard provisions domain controllers in two Availability Zones to provide highly available, low latency access to AD DS services in the AWS Cloud\.

Launch Wizard can build a new VPC for the deployment, or deploy into an existing VPC\. To accommodate highly available AD DS in the AWS Cloud, Launch Wizard builds \(or requires, in the case of existing VPCs\) a base Amazon VPC configuration that complies with the following AWS best practices: 
+ Domain controllers must be placed in a minimum of two Availability Zones to provide high availability\. 
+ Domain controllers and other non\-internet facing servers must be placed in private subnets\. 
+ Launched instances require internet access to connect to the AWS CloudFormation endpoint during the bootstrapping process\. To support this configuration, public subnets are used to host NAT gateways for outbound internet access\. Remote Desktop Gateways are also deployed into the public subnets for remote administration\. Other components such as reverse proxy servers can be placed into these public subnets, if needed\. 

This VPC architecture uses two Availability Zones, each with its own distinct public and private subnets\. We recommend that you leave plenty of unallocated address space to support the growth of your environment over time and to reduce the complexity of your VPC subnet design\. Launch Wizard uses a default VPC configuration that provides plenty of address space by using the minimum number of private and public subnets\. By default, Launch Wizard uses the following CIDR ranges\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-ad-implementation.html)

In addition, Launch Wizard provisions spare capacity for additional subnets to support your environment as it grows or changes over time\. If you have sensitive workloads that must be completely isolated from the internet, you can create new VPC subnets using these optional address spaces\.

## Security groups<a name="launch-wizard-ad-implementation-sg"></a>

Amazon EC2 instances must be associated with a security group, which acts as a stateful firewall\. You control the network traffic entering or leaving the security group, and you can create rules that are defined by protocol, port number, and source/destination IP address, or other security groups\. By default, all egress traffic from a security group is permitted\. However, ingress traffic must be configured to allow the desired traffic to reach your instances\. 

The [Securing the Microsoft Platform on Amazon Web Services whitepaper](https://d1.awsstatic.com/whitepapers/aws-microsoft-platform-security.pdf?trk=wp_card) explains the different methods for securing your AWS infrastructure\. Recommendations include providing isolation between application tiers by using security groups\. We recommend that you tightly control ingress traffic in order to reduce the attack surface of your Amazon EC2 instances\.

If you are deploying and managing your own AD DS installation, domain controllers and member servers will require several security group rules to allow traffic for services\. These rules include AD DS replication, user authentication, Windows Time services, and Distributed File System \(DFS\)\. You should also consider restricting these rules to specific IP subnets that are used within your VPC\. 

For a detailed list of port mappings used by AWS CloudFormation, see the [Security best practices ](launch-wizard-ad-best-practices.md#launch-wizard-ad-security) in this guide\.

For a complete list of ports, see [Active Directory and Active Directory Domain Services Port Requirements](http://technet.microsoft.com/library/dd772723(v=ws.10).aspx) in the Microsoft TechNet Library and [How to configure a firewall for Active Directory domains and trusts](https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/config-firewall-for-ad-domains-and-trusts) for forest trusts\. For guidance on implementing rules, see [Adding Rules to a Security Group](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html#adding-security-group-rule) in the *Amazon EC2 User Guide*\.

## Remote Desktop Gateway<a name="launch-wizard-ad-implementation-rdg"></a>

When you design your architecture for highly available AD DS, you should also design for highly available and secure remote access\. Launch Wizard optionally allows for deployment of a Remote Desktop \(RD\) Gateway server to manage your AD DS instances\.

RD Gateway uses the Remote Desktop Protocol \(RDP\) over HTTPS to establish a secure, encrypted connection between remote administrators on the internet and Windows\-based Amazon EC2 instances, without the need for a virtual private network \(VPN\) connection\. This configuration reduces the attack surface of your Windows\-based Amazon EC2 instances, while providing a remote administration solution for administrators\. 

**Important**  
Never open up RDP to the entire internet even temporarily or for testing purposes\. Always restrict ports and source traffic to the minimum necessary to support the functionality of the application\.

## Active Directory<a name="launch-wizard-ad-implementation-ad"></a>

This section provides information about key design considerations specific to a Launch Wizard deployment of Active Directory Domain Services \(AD DS\) domain controllers on AWS\.

**Topics**
+ [Highly available directory domain services](#launch-wizard-ad-implementation-domain-controllers)
+ [Active Directory DNS and DHCP inside the VPC](#launch-wizard-ad-implementation-dns-dhcp)
+ [DNS settings on Windows Servers instances](#launch-wizard-ad-dns-settings)
+ [Active Directory Certificate Services](#launch-wizard-ad-adcs)

### Highly available directory domain services<a name="launch-wizard-ad-implementation-domain-controllers"></a>

Launch Wizard deploys two domain controllers in your AWS environment in two Availability Zones\. This design provides fault tolerance and prevents a single domain controller failure from affecting the availability of the AD DS\. 

To strengthen the high availability of your architecture and help mitigate the impact of a possible disaster, each domain controller deployed by Launch Wizard is a global catalog server and an Active Directory DNS server\. 

When you choose to deploy self\-managed domain controllers to the AWS Cloud, Launch Wizard automatically builds out an Active Directory Sites and Services configuration that supports a highly available AD DS architecture\.

For information about creating sites, adding global catalog servers, and creating and managing site links, see the [Microsoft Active Directory Sites and Services](http://technet.microsoft.com/library/cc730868.aspx) documentation\.

### Active Directory DNS and DHCP inside the VPC<a name="launch-wizard-ad-implementation-dns-dhcp"></a>

Dynamic Host Configuration Protocol \(DHCP\) services are provided by default for your instances within a VPC\. DHCP scopes do not need to be managed; they are created for the VPC subnets you define when you deploy your solution\. These DHCP services cannot be disabled, so you must use them rather than deploying your own DHCP server\. 

The VPC also provides an internal DNS server\. This DNS provides instances with basic name resolution services for internet access and is crucial for access to AWS service endpoints, such as AWS CloudFormation and Amazon S3 during bootstrapping\.

Amazon\-provided DNS server settings will be assigned to instances launched into the VPC based on a DHCP options set\. DHCP options sets are used within an Amazon VPC to define scope options, such as the domain name or the name servers that should be handed to your instances via DHCP\. Amazon\-provided DNS is used only for public DNS resolution\. 

Because Amazon\-provided DNS cannot be used to provide name resolution services for Active Directory, you must ensure that domain\-joined Windows instances are configured to use Active Directory DNS\. 

Launch Wizard statically assigns Active Directory DNS server addresses on Windows instances\. You can alternatively specify them using a custom DHCP options set\. This allows you to assign your Active Directory DNS suffix and DNS server IP addresses as the name servers within the VPC through DHCP\.

**Note**  
The IP addresses in the `domain-name-servers` field are always returned in the same order\. If the first DNS server in the list fails, instances should fall back to the second IP and continue to resolve host names successfully\. However, during normal operations, the first DNS server listed will always handle DNS requests\. If you want to ensure that DNS queries are distributed evenly across multiple servers, you should consider statically configuring DNS server settings on your instances\.

For more information about creating a custom DHCP options set and associating it with your VPC, see [Working with DHCP Options Sets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html#DHCPOptionSet) in the *Amazon VPC User Guide*\.

**Note**  
If you choose to deploy self\-managed domain controllers in the AWS Cloud, Launch Wizard adds the DNS suffix for your domain to the DNS suffixes list\. The DNS settings on the local server point to the IP address of the first domain controller for all of the domain controllers in the infrastructure\.

### DNS settings on Windows Servers instances<a name="launch-wizard-ad-dns-settings"></a>

To ensure that domain\-joined Windows instances automatically register host \(A\) and reverse lookup \(PTR\) records with Active Directory\-integrated DNS, set the properties of the network connection as shown in the following image\. 

![\[Advanced TCP/IP settings on a domain-joined Windows instance\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/tcp-ip.png)

The default configuration for a network connection is set to automatically register the connections address in DNS\. In other words, the **Register this connection's addresses in DNS** option is selected for you automatically\. This takes care of host \(A\) record dynamic registration\. However, if you do not also select the second option, **Use this connection's DNS suffix in DNS registration**, dynamic registration of PTR records will not occur\.

If you have a small number of instances in the VPC, you may choose to manually configure the network connection\. For larger fleets, you can push this setting out to all of your Windows instances by using Active Directory Group Policy\. For instructions about how to do this, see [IPv4 and IPv6 Advanced DNS Tab](http://technet.microsoft.com/library/cc754143.aspx) in the Microsoft TechNet Library\.

### Active Directory Certificate Services<a name="launch-wizard-ad-adcs"></a>

Launch Wizard sets up and deploys Active Directory Certificate Services \(AD CS\) with a new Active Directory infrastructure to issue and manage digital certificates in systems that use public key technologies\. For more information about AD CS, see the [Microsoft documentation](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831740(v=ws.11)#role-description)\.

## Self\-managed domain controller architecture<a name="launch-wizard-ad-architecture"></a>

The Launch Wizard self\-managed domain controller deployment sets up the following architecture\.
+ Domain controllers are deployed into two private VPC subnets in separate Availability Zones, which makes AD DS highly available\.
+ NAT gateways are deployed to public subnets, providing outbound internet access for instances in private subnets\.
+ Remote Desktop gateways are deployed in an Auto Scaling group in one Availability Zone to allow access to the domain controllers\.

The specified version of Windows Server is used for the Remote Desktop Gateway instances and the domain controller instances\. Launch Wizard deploys AWS resources, including a Systems Manager Automation document\. When the second node is deployed, it initiates running the Automation document through Amazon EC2 user data\. The automation workflow deploys the required components, finalizes the configuration to create a new AD forest, and promotes instances in two Availability Zones to Active Directory domain controllers\.

To view architectural diagrams showing best practices for setting up an AD DS environment, see [Active Directory Domain Services on AWS](http://aws.amazon.com/quickstart/architecture/active-directory-ds/)\.
