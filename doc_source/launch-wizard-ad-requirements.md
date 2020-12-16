# Requirements<a name="launch-wizard-ad-requirements"></a>

Your account must be configured as specified in the following table to deploy self\-managed domain controllers using Launch Wizard\.

To add domain controllers to an existing infrastructure, you must create a [VPC peering](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html) connection between the two VPCs for an existing Active Directory in AWS\. If you are using an existing Active Directory on premises, you must use [AWS Direct Connect](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html)\. To ensure that instances in the VPCs can communicate with each other, you can use either Direct Connect or [VPC Private Link](https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-services-overview.html)\. For more information about VPC connectivity, see [VPN connections](https://docs.aws.amazon.com/vpc/latest/userguide/vpn-connections.html)\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-ad-requirements.html)

If you have an existing environment that uses these resources and you think that deploying domain controllers in this environment using Launch Wizard may exceed your default quotas, you can [request service quota increases](https://console.aws.amazon.com/servicequotas) for these resources\. For default quotas, see [AWS service quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\.

For additional prerequisites to deploy domain controllers using Launch Wizard, see [Set up for AWS Launch Wizard for Active Directory](launch-wizard-ad-setting-up.md)\. 
