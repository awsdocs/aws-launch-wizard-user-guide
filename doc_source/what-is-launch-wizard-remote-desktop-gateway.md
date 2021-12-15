# What is AWS Launch Wizard for Remote Desktop Gateway?<a name="what-is-launch-wizard-remote-desktop-gateway"></a>

AWS Launch Wizard for Remote Desktop Gateway \(RD Gateway\) guides you through the sizing, configuration, and deployment of RD Gateway on the AWS Cloud\. RD Gateway uses the Remote Desktop Protocol \(RDP\) over HTTPS to establish a secure, encrypted connection between remote users and Amazon Elastic Compute Cloud instances running Windows, without needing to configure a virtual private network \(VPN\)\. This helps reduce the attack surface on your Windows instances while providing a remote administration solution for administrators\.

**Topics**
+ [Deployment options](#launch-wizard-remote-desktop-gateway-deployment-types)
+ [Features](launch-wizard-remote-desktop-gateway-features.md)
+ [Components](launch-wizard-remote-desktop-gateway-components.md)

## Deployment options<a name="launch-wizard-remote-desktop-gateway-deployment-types"></a>

This Launch Wizard application provides two deployment options:
+ **Deploy RD Gateway into a new VPC \(end\-to\-end deployment\)\.** Builds a new AWS environment consisting of a VPC, subnets, NAT gateways, security groups, and other infrastructure components, and then deploys RD Gateway into this new VPC\. 
+ **Deploy RD Gateway into an existing VPC\.** Provisions standalone RD Gateway instances in your existing AWS infrastructure\.

AWS Launch Wizard provides separate templates for these two deployment types\. You can also configure CIDR blocks, instance types, and RD Gateway settings\.