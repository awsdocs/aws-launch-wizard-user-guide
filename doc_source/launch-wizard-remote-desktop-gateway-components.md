# Components<a name="launch-wizard-remote-desktop-gateway-components"></a>

An RD Gateway application deployed with Launch Wizard includes the following components:
+ A highly available architecture that spans two Availability Zones\.
+ In each public subnet, up to four RD Gateway instances in an Auto Scaling group to provide secure remote access to instances in the private subnets\. Each instance is assigned an Elastic IP address so it’s reachable directly from the internet\.
+ A Network Load Balancer to provide RDP access to the RD Gateway instances\.
+ A security group for Windows instances that will host the RD Gateway role, with an ingress rule permitting TCP port 3389 from your administrator IP address\. After deployment, you’ll modify the security group ingress rules to configure administrative access through TCP port 443 instead\.
+ An empty application tier for instances in private subnets\. If more tiers are required, you can create additional private subnets with unique CIDR ranges\.
+ AWS Systems Manager Parameter Store to securely store credentials used for accessing the RD Gateway instances\.
+ AWS Systems Manager to automate the deployment of the RD Gateway Auto Scaling group\.
+  Self\-signed SSL certificate and configuration of Remote Desktop Connection Authorization Policies \(RD CAPs\) and RD Gateway\.
+ Resource Groups that contain all the resources created with Launch Wizard\.

Additionally, a new VPC deployment includes the following components:
+ A VPC configured with public and private subnets according to AWS best practices, to provide you with your own virtual network on AWS\.
+ An internet gateway to allow access to the internet\. This gateway is used by the RD Gateway instances to send and receive traffic\.
+ Managed network address translation \(NAT\) gateways to allow outbound internet access for resources in the private subnets\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/rdgateway-architecture-diagram.png)