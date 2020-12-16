# Set up for AWS Launch Wizard for Active Directory<a name="launch-wizard-ad-setting-up"></a>

Verify the following prerequisites to deploy self\-managed domain controllers with AWS Launch Wizard\.

**Topics**
+ [Active Directory](#launch-wizard-ad-setup)
+ [AWS Identity and Access Management \(IAM\)](#launch-wizard-ad-iam)
+ [Requirements for using custom AMIs](#launch-wizard-ad-custom-ami)
+ [Configuration settings](#launch-wizard-ad-config)

## Active Directory<a name="launch-wizard-ad-setup"></a>

This section contains information to set up for deployment of domain controllers into an existing VPC and for deployment into an existing, on\-premises Active Directory\. It also contains information for setting up forest trusts\. 

### Active Directory on EC2<a name="launch-wizard-ad-setup-managed"></a>

If you deploy domain controllers into an existing VPC with an existing Active Directory, Launch Wizard requires domain administrator credentials to be added to Secrets Manager to join your domain controllers to Active Directory and promote them\. In addition, a resource policy must be attached to the secret so that Launch Wizard can access the secret\. Launch Wizard guides you through the following process of attaching the required policy to your secret\.

**How to attach the resource policy to your secret so that Launch Wizard can access the secret**

1. Navigate to the [Secrets Manager console](https://console.aws.amazon.com/secretsmanager)\.

1. Under **Secret name**, choose the name of your secret\.

1. Under **Resource permissions**, choose **Edit permissions**\.

1. Copy the following policy and paste it into the **Resource permissions** box, entering your account ID in the path of the IAM ARN\.

   ```
   {
       "Version" : "2012-10-17",
       "Statement" : [ {
           "Effect" : "Allow",
           "Principal" : {
               "AWS" :
               "arn:aws:iam::<account-id>:role/service-role/AmazonEC2RoleForLaunchWizard"
           },
           "Action" : [
               "secretsmanager:GetSecretValue",
               "secretsmanager:CreateSecret",
               "secretsmanager:GetRandomPassword"
           ],
           "Resource" : "*"
           } 
       ]
   }
   ```

1. Choose **Save**\.

1. Resume your Launch Wizard deployment\.

The following key operations are performed against your Active Directory by Launch Wizard\. These operations result in the creation of new records or entries in Active Directory\.
+ Creates a new Amazon EC2 instance and joins it to the domain\.
+ Creates ingress and egress rules to communicate with your domain controllers\.
+ Promotes the server to a domain controller in your domain\.
+ Updates local DNS on the new domain controllers to point to your DNS server\.

### On\-premises Active Directory through AWS Direct Connect<a name="launch-wizard-ad-setup-onprem"></a>

If you are deploying domain controllers into an existing VPC and connecting to an on\-premises Active Directory, ensure that the following prerequisites are in place\.
+ Make sure that you have connectivity between your AWS account and your on\-premises network\. You can establish a dedicated network connection from your on\-premises network to your AWS account with AWS Direct Connect\. For more information, see [the AWS Direct Connect documentation](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html)\. 
+ The domain functional level of your Active Directory domain controller must be Windows Server 2012 or later\.
+ The IP addresses of your DNS server must be either in the same VPC CIDR range as the one in which your Launch Wizard domain controllers will be created, or in the private IP address range\. 
+ The firewall on the Active Directory domain controllers should allow the connections from the Amazon VPC from which you will create the Launch Wizard deployment\. At a minimum, your configuration should include the ports mentioned in [How to configure a firewall for Active Directory domains and trusts](https://support.microsoft.com/en-us/help/179442/how-to-configure-a-firewall-for-domains-and-trusts)\.

You can optionally perform the following step\.
+ Establish DNS resolution across your environments\. For options on how to set this up, see [ How to Set Up DNS Resolution Between On\-Premises Networks and AWS Using AWS Directory Service and Amazon Route 53](https://aws.amazon.com/blogs/security/how-to-set-up-dns-resolution-between-on-premises-networks-and-aws-using-aws-directory-service-and-amazon-route-53/) or [How to Set Up DNS Resolution Between On\-Premises Networks and AWS Using AWS Directory Service and Microsoft Active Directory](https://aws.amazon.com/blogs/security/how-to-set-up-dns-resolution-between-on-premises-networks-and-aws-using-aws-directory-service-and-microsoft-active-directory/)\.

### Trust relationships<a name="launch-wizard-ad-setup-trusts"></a>

If you are creating a forest trust relationship, you must complete the [prerequisites for AWS Managed Active Directory](#launch-wizard-ad-setup-managed) before you set up the trust\. For more information about creating forest trust relationships, see [Configure forest trust relationships](launch-wizard-ad-create-trusts.md)\.

## AWS Identity and Access Management \(IAM\)<a name="launch-wizard-ad-iam"></a>

The following steps establish the required AWS Identity and Access Management \(IAM\) role and set up the IAM user for permissions\.

### One\-time creation of IAM Role<a name="launch-wizard-ad-iam-role"></a>

On the **Choose Application** page of Launch Wizard, under **Permissions**, Launch Wizard displays the IAM role required for the Amazon EC2 instances that have been created by Launch Wizard to access other AWS services on your behalf\. When you select **Next**, Launch Wizard attempts to discover the IAM role in your account\. If the role exists, the role is attached to the instance profile for the Amazon EC2 instances that Launch Wizard will launch into your account\. If the role does not exist, Launch Wizard attempts to create the role with the same name, `AmazonEC2RoleForLaunchWizard`\. This role is comprised of two IAM managed policies: `AmazonSSMManagedInstanceCore` and `AmazonEC2RolePolicyForLaunchWizard`\. After the role is created, the IAM Administrator can delegate the application deployment process to another IAM user who, in turn, must have the Launch Wizard IAM managed policy described in the following section\.

### IAM user setup<a name="launch-wizard-ad-iam-user-setup"></a>

To deploy self\-managed domain controllers with Launch Wizard, you must create an [Identity and Access Management \(IAM\) policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) and attach it to your IAM user identity\. The IAM policy defines the user permissions\. If you do not already have an IAM user in your account, follow the steps listed in [Create an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)\.

When you have an IAM user in your account, create an IAM policy\.

1. Go to the IAM console at [ https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam)\. In the left navigation pane, choose **Policies**\.

1. Choose **Users** from the left navigation pane\. 

1. Select the **User name** of the user to which you want to attach the policy\.

1. Select **Add permissions**\. 

1. Select **Attach existing policies directly**\. 

1. Search for the policy named **AmazonLaunchWizard\_Fullaccess**, and select the check box to the left of the policy name\.

1. Select **Next: Review**\. 

1. Verify that the correct policy is listed, and then select **Add permissions**\. 

**Important**  
Make sure that you log in with the user associated with the policy described in this procedure when you use Launch Wizard\. 

## Requirements for using custom AMIs<a name="launch-wizard-ad-custom-ami"></a>

We recommend that you use license\-included Windows AMIs whenever possible\. There are scenarios for which you may want to use a custom Windows AMI\. For example, you may have existing licenses \(BYOL\), or you may have made changes to one of our public images and re\-imaged it\.

If you use license\-included Windows AMIs, you are not required to perform any pre\-checks on the AMI to ensure that it meets Launch Wizard requirements\.

Launch Wizard relies on user data to begin the process of configuring domain controller instances that the service launches in your account\. For more information, see [User Data Scripts](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-windows-user-data.html)\. By default, all Windows AMIs have user data execution enabled for the initial launch\. To ensure that your custom AMIs are set up to run the User Data script at launch, follow the AWS\-recommended method to prepare your AMIs using [EC2Launch v2](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2launch-v2.html)\. For more information about how to prepare your custom AMI using the options to Shutdown with Sysprep or Shutdown without Sysprep, see [Create a Standard Amazon Machine Image Using Sysprep](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/Creating_EBSbacked_WinAMI.html#ami-create-standard) or [EC2Launch v2 and Sysprep](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2launch.html#ec2launch-v2-sysprep)\. If you want to directly enable user data as part of the custom AMI creation process, follow the steps for Subsequent Reboots or Starts under [Running Commands on Your Windows Instance at Launch](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-windows-user-data.html)\. 

If you use a custom AMI, the volume drive letter for the root partition should be `C:`, because EC2Launch v2 and EC2Config rely on this configuration to install the components\. 

While not exhaustive, the following requirements cover most of the configurations whose alteration might impact the successful deployment of a domain controllers using Launch Wizard\.


| Windows Server 2016 | Windows Server 2019 | 
| --- | --- | 
|  YES  |  YES  | 
|  YES  |  YES  | 
| YES | YES | 

**Operating system requirements**
+ Windows Server 2016 
+ Windows Server 2019
+ English language pack only 
+ The root volume drive for the custom AMI should be C:

**AWS software and drivers**
+ AWS CloudFormation `cfn-init` script \(See [Bootstrapping AWS CloudFormation Windows stacks](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-windows-stacks-bootstrapping.html)\.
+ EC2Launch \(Windows Server 2016\)
+ AWS Systems Manager \([SSM agent must be installed](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-install-win.html)\)
+ AWS Tools for Windows PowerShell
+ Network drivers \(SRIOV, ENA\)
+ Storage drivers \(NVMe, AWS PV\)

## Configuration settings<a name="launch-wizard-ad-config"></a>

The following configuration settings are applied when deploying self\-managed domain controllers with Launch Wizard\.


| Setting | Applies to | 
| --- | --- | 
|  Current EC2Launch and SSM Agent  |  Windows Server 2016 and Windows Server 2019  | 
|  Current AWS PV, ENA, and NVMe drivers  |  Windows Server 2016 and Windows Server 2019  | 
|  Current SRIOV drivers  |  Windows Server 2016 and Windows Server 2019  | 
|  Allow ICMP traffic through the firewall  |  Windows Server 2016 and Windows Server 2019  | 
|  Allow RDP traffic through host firewall  |  Windows Server 2016 and Windows Server 2019  | 
|  RealTimeIsUniversal registry key set  |  Windows Server 2016 and Windows Server 2019  | The following AMI settings can impact the Launch Wizard deployment:

**System Time**  
**RealTimeIsUniversal**\. If disabled, Windows system time drifts when the time zone is set to a value other than UTC\.

**Windows Firewall**  
In most cases, Launch Wizard configures the correct protocols and ports\. However, custom Windows Firewall rules could impact the cluster service\. To ensure that your custom AMI works with Launch Wizard, see [Service overview and network port requirements for Windows](https://support.microsoft.com/en-us/help/832017/service-overview-and-network-port-requirements-for-windows)\.

**Remote Desktop**  
**Service Start**\. Remote Desktop service must be enabled\.  
**Remote Desktop Connections**\. Must be enabled\.

**Network Interface**  
**DHCP Service Startup**\. DHCP service should be enabled\.  
**DHCP on Ethernet**\. DHCP should be enabled\.

**PowerShell**  
**Execution Policy**\. The execution policy in all AWS license\-included AMIs is set to `Unrestricted`\. We recommend that you set this policy to `Unrestricted` when you set up domain controllers using AWS Launch Wizard You can change the policy when setup is complete\. 
