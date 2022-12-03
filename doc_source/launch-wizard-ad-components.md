# Components<a name="launch-wizard-ad-components"></a>

Self\-managed domain controllers deployed with Launch Wizard include the following components:
+ A **virtual private cloud \(VPC\)** configured with [public and private subnets](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html#what-is-vpc-subnet) across two Availability Zones\. A public subnet is a subnet whose traffic is routed to an internet gateway\. If a subnet does not have a route to the internet gateway, then it is a private subnet\. The VPC provides the network infrastructure for your domain controller environment\.
+ **Amazon EC2 instances** on which to provision your domain controllers\.
+ An **internet gateway** to provide access to the internet\.
+ In the public subnets, **network address translation \(NAT\) gateways** for outbound internet access\. If you are deploying in your preexisting VPC, Launch Wizard uses the existing NAT gateway in your VPC\. For more information about NAT gateways, see [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)\.
+ **Elastic IP addresses** associated with the NAT gateway and RDGW instances\. For more information about Elastic IP addresses, see [Elastic IP Addresses](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/elastic-ip-addresses-eip.html)\.
+ **AWS CloudFormation** templates and **PowerShell** configuration scripts to perform the domain controller configuration steps\.
+ **Security groups** to ensure the secure flow of traffic between the instances deployed in the VPC\. For more information, see [Security Groups for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)\.
+ **AWS Secrets Manager** to protect secrets required to generate and store your Active Directory Administrator credentials\. 
+ **Amazon CloudWatch Logs** to monitor, store, and access your log files produced by AWS CloudFormation\.
+ Amazon Kinesis Agent for Microsoft Windows to gather, parse, transform, and stream logs, events, and metrics to Amazon CloudWatch Logs\. For more information, see [What Is Amazon Kinesis Agent for Microsoft Windows?](https://docs.aws.amazon.com/kinesis-agent-windows/latest/userguide/what-is-kinesis-agent-windows.html)