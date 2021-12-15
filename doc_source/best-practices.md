# Best practices<a name="best-practices"></a>

Best practices for using Remote Desktop Gateway on AWS

**Topics**
+ [The Principle of Least Privilege](#least-privilege)
+ [VPC Configuration](#vpc-config)
+ [Network Access Control Lists](#nacl)
+ [Security groups](#security-groups)
+ [Initial Remote Administration Architecture](#remote-admin-arch)
+ [SSL Certificates](#ssl-certs)
+ [Connection and Resource Authorization Policies](#connect-resource-auth-policy)

## The Principle of Least Privilege<a name="least-privilege"></a>

When considering remote administrative access to your environment, it is important to follow the principle of least privilege\. This principle refers to users having the fewest possible permissions necessary to perform their job functions\. This helps reduce the attack surface of your environment, making it much harder for an adversary to exploit\. An attack surface can be defined as the set of exploitable vulnerabilities in your environment, including the network, software, and users who are involved in the ongoing operation of the system\.

Following the principle of least privilege, we recommend reducing the attack surface of your environment by exposing the absolute minimal set of ports to the network while also restricting the source network or IP address that will have access to your Amazon Elastic Compute Cloud instances\.

In addition to the functionality that exists in the Windows platform, there are several AWS capabilities to help you implement the principle of least privilege, such as subnets, security groups, and trusted ingress CIDR blocks\.

## VPC Configuration<a name="vpc-config"></a>

Amazon Virtual Private Cloud lets you provision a private, isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define\. With Amazon VPC, you can define a virtual network topology closely resembling a traditional network that you might operate on your own premises\. You control your virtual networking environment\. This includes the selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways\.

**When deploying Windows architecture on the AWS Cloud, we recommend a VPC configuration that supports the following requirements:**
+ Place critical workloads in a minimum of two Availability Zones to provide high availability\. 
+ Place instances into individual tiers\. For example, in a Microsoft SharePoint deployment, you should have separate tiers for web servers, application servers, database servers, and domain controllers\. Traffic between these groups can be controlled to adhere to the principle of least privilege\.
+ Deploy RD Gateways into public subnets in each Availability Zone for remote administration\. Other components, such as reverse proxy servers, can also be placed into these public subnets if needed\.

## Network Access Control Lists<a name="nacl"></a>

A network access control list \(ACL\) is a set of permissions that you can attach to any network subnet in a VPC to provide stateless filtering of traffic\. You can use network ACLs for inbound or outbound traffic, as they provide an effective way to place a CIDR block or individual IP addresses on a deny list\. These ACLs can contain ordered rules to allow or deny traffic based on IP protocol, service port, or source or destination IP address\. The following image shows the default ACL configuration for a VPC subnet, which is also used by this Launch Wizard deployment:

![\[Default network ACL configuration for a VPC subnet.\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/default-nacl.png)

 You can keep the default network ACL configuration, or you can configure more specific rules to restrict traffic between subnets at the network level\. For example, you could set a rule that would allow inbound administrative traffic on TCP port 3389 from a specific set of IP addresses\. In either case, you must implement security group rules to permit access from users connecting to RD Gateways and between tiered groups of Amazon EC2 instances\.

## Security groups<a name="security-groups"></a>

All instances are required to belong to one or more security groups\. Security groups allow you to set policies to control open ports and provide isolation between application tiers\. In a VPC, every instance runs behind a stateful firewall with all ports closed by default\. The security group contains rules responsible for opening inbound and outbound ports on that firewall\. While security groups act as an instance\-level firewall, they can also be associated with multiple instances, providing isolation between application tiers in your environment\. For example, you can create a security group for all your web servers that will allow traffic on TCP port 3389, but only from members of the security group containing your RD Gateway servers\. The following diagram illustrates this configuration:

![\[Security groups for RD Gateway administrative access.\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/security-group-admin.png)

Notice that inbound connections from the internet are only permitted over TCP port 443 to the RD Gateways\. The RD Gateways have an Elastic IP address assigned and have direct access to the internet\. The remaining Windows instances are deployed into private subnets and are assigned private IP addresses only\. Security group rules allow only the RD Gateways to initiate inbound connections for remote administration to TCP port 3389 for instances in the private subnets\.

In this architecture, RDP connections are established over HTTPS to the RD Gateway and proxied to backend instances on the standard RDP TCP port 3389\. This configuration helps you reduce the attack surface on your Windows instances while allowing administrators to establish connections to all your instances through a single gateway\.

It’s possible to provide remote administrative access to all your Windows instances through one RD Gateway, but we recommend placing gateways in each Availability Zone for redundancy\. This Launch Wizard deployment implements this best practice for you\.

## Initial Remote Administration Architecture<a name="remote-admin-arch"></a>

In an initial RD Gateway configuration, the servers in the public subnet will need an inbound security group rule permitting TCP port 3389 from the administrator’s source IP address or subnet\. Windows instances sitting behind the RD Gateway in a private subnet will be in their own isolated tier\. For example, a group of web server instances in a private subnet may be associated with their own web tier security group\. This security group will need an inbound rule allowing connections from the RD Gateway on TCP port 3389\.

Using this architecture, an administrator can use a traditional RDP connection to an RD Gateway to configure the local server\. The RD Gateway can also be used as a bastion host \(jump box\)\. This means that when an RDP connection is established to the desktop of the RD Gateway, an administrator can start a new RDP client session to initiate a connection to an instance in a private subnet, as illustrated in the following diagram:

![\[Initial architecture for remote administration\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/initial-arch.png)

Although this architecture works well for initial administration, it is not recommended for the long term\. To further secure connections and reduce the number of RDP sessions required to administer the servers in the private subnets, the inbound rule should be changed to permit TCP port 443\. The RD Gateway service should be installed and configured with an SSL certificate and Remote Desktop Connection Authorization Policies \(RD CAP\)\.

This Launch Wizard deployment sets up a standard TCP port 3389 connection from the administrator’s IP address\. You must follow the post\-deployment steps to modify the security group for RD Gateway to use a single inbound rule permitting TCP port 443\. This modification will allow a Transport Layer Security \(TLS\) encrypted RDP connection to be proxied through the gateway over TCP port 443 directly to one or more Windows instances in private subnets on TCP port 3389\. This configuration increases the security of the connection and also prevents the need to initiate an RDP session to the desktop of the RD Gateway\. The following diagram illustrates this configuration:

![\[Architecture for RD Gateway administrative access\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/admin-arch.png)

## SSL Certificates<a name="ssl-certs"></a>

**The RD Gateway role uses Transport Layer Security \(TLS\) to encrypt communications over the internet between administrators and gateway servers\. To support TLS, a valid X\.509 SSL certificate must be installed on each RD Gateway\. Certificates can be acquired in a number of ways, including:**
+ Your own PKI infrastructure, such as a Microsoft Enterprise Certificate Authority \(CA\)
+ Certificates issued by a public CA, such as Verisign or Digicert
+ Self\-signed certificates

For smaller test environments, implementing a self\-signed certificate is a straightforward process that helps you get up and running quickly\. This Launch Wizard deployment automatically generates a self\-signed certificate for RD Gateway\.

However, if you have a large number of varying administrative devices that need to establish a connection to your gateways, we recommend using a public certificate\.

**For an RDP client to establish a secure connection with an RD Gateway, the following certificate and DNS requirements must be met:**
+ The issuing CA of the certificate installed on the gateway must be trusted by the RDP client\. For example, the root CA certificate must be installed in the client machine’s Trusted Root Certification Authorities store\.
+ The subject name used on the certificate installed on the gateway must match the DNS name used by the client to connect to the server; for example, rdgw1\.example\.com\.
+ The client must be able to resolve the hostname \(for example, rdgw1\.example\.com\) to the Elastic IP address of the RD Gateway\. This will require a Host \(A\) record in DNS\.

There are various considerations when choosing the right CA to obtain an SSL certificate\. For example, a public certificate may be ideal, because the issuing CA will be widely trusted by the majority of client devices that need to connect to your gateways\. However, you may want to use your own PKI infrastructure to ensure that only the machines that are part of your organization will trust the issuing CA\.

## Connection and Resource Authorization Policies<a name="connect-resource-auth-policy"></a>

**Users must meet specific requirements to connect to RD Gateway instances:**
+ *Connection Authorization Policies* – Remote Desktop Connection Authorization Policies \(RD CAPs\) allow you to specify who can connect to an RD Gateway instance\. For example, you can select a group of users from your domain, such as Domain Admins\.
+ *Resource Authorization Policies* – Remote Desktop Resource Authorization Policies \(RD RAPs\) allow you to specify the internal Windows instances that remote users can connect to through an RD Gateway instance\. For example, you can choose specific computers joined to a domain, which administrators can connect to through the RD Gateway\.

This Launch Wizard deployment automatically sets up Connection and Resource Authorization Policies\.