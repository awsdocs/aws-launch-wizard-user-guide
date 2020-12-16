# High availability and security best practices for AWS Launch Wizard for Active Directory<a name="launch-wizard-ad-best-practices"></a>

The domain controller architecture created by AWS Launch Wizard supports AWS best practices for high availability and security as promoted by the [AWS Well\-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)\.

**Topics**
+ [High availability](#launch-wizard-ad-ha)
+ [Security in Launch Wizard for Active Directory](#launch-wizard-ad-security)

## High availability<a name="launch-wizard-ad-ha"></a>

With Amazon EC2, you can set the location of instances in multiple locations composed of AWS Regions and Availability Zones\. Regions are dispersed and located in separate geographic areas\. Availability Zones are distinct locations within a Region that are engineered to be isolated from failures in other Availability Zones\. Availability Zones provide inexpensive, low\-latency network connectivity to other Availability Zones in the same Region\.

When you launch your instances in different Regions, you can set your domain controllers to be closer to specific customers, or to meet legal or other requirements\. When you launch your instances in different Availability Zones, you can protect your domain controllers from the failure of a single location\. 

## Security in Launch Wizard for Active Directory<a name="launch-wizard-ad-security"></a>

Launch Wizard creates a number of security groups and rules for you\. When Amazon EC2 instances are launched, they must be associated with a security group, which acts as a stateful firewall\. You have complete control over the network traffic entering or leaving the security group\. You can also build granular rules that are scoped by protocol, port number, and source or destination IP address or subnet\. By default, all outbound traffic from a security group is permitted\. Inbound traffic, on the other hand, must be configured to allow the appropriate traffic to reach your instances\. 

The [Securing the Microsoft Platform on Amazon Web Services](https://d1.awsstatic.com/whitepapers/aws-microsoft-platform-security.pdf) whitepaper discusses the different methods for securing your AWS infrastructure\. Recommendations include providing isolation between application tiers using security groups\. We recommend that you tightly control inbound traffic to reduce the attack surface of your EC2 instances\.

Launch Wizard configures the necessary security groups for you, which are listed in the following table\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-ad-best-practices.html)

**Important**  
Always restrict ports and source traffic to the minimum necessary to support the functionality of the application\. 
