# Set up for AWS Launch Wizard for SQL Server<a name="launch-wizard-setting-up"></a>

**Topics**
+ [Active Directory \(Windows\)](#launch-wizard-ad)
+ [IAM](#launch-wizard-iam)
+ [AWS Secrets Manager](#launch-wizard-sql-prerequisites-secrets-manager)
+ [Custom AMIs \(Windows\)](#launch-wizard-custom-ami)
+ [Custom AMIs \(Linux\)](#launch-wizard-custom-ami-linux)
+ [Windows license\-included AMIs](#launch-wizard-sql-prerequisites-license-included-ami)
+ [Amazon FSx](#launch-wizard-sql-prerequisites-fsx)
+ [Configuration settings \(deployment on Windows\)](#launch-wizard-config)

## Active Directory \(Windows deployment\)<a name="launch-wizard-ad"></a>

### AWS Managed Active Directory<a name="launch-wizard-ad-managed"></a>

If you are [deploying SQL Server into an existing VPC with an existing Active Directory](launch-wizard-deployment-options.md#option-3), Launch Wizard uses your Managed Active Directory \(AD\) domain user credentials to set up a fully functional SQL Server Always On Availability Group in the Active Directory\. Launch Wizard supports this deployment option only for AWS Managed Active Directory\. Your Managed Active Directory does not have to be in the same VPC as the one in which SQL Server Always On is deployed\. If it is in a different VPC than the one in which SQL Server Always On is deployed, verify that you set up connectivity between the two VPCs\. The domain user requires the following permissions in the [Active Directory Default organizational unit \(OU\)](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/creating-an-organizational-unit-design) to enable Launch Wizard to perform the deployment successfully:
+ `Reset password`
+ `Write userAccountControl`
+ `Create user accounts`
+ `Create computer objects`
+ `Read all properties`
+ `Modify permissions`

The following key operations are performed against your Active Directory by Launch Wizard\. These operations result in the creation of new records or entries in Active Directory\.
+ SQL Server service user added as a new Active Directory user if it does not already exist in Active Directory\.
+ SQL Server instance and Remote Desktop Gateway Access instance joined to the Active Directory domain\.
+ `CreateChild` role added to Windows Server Failover Cluster as part of `ActiveDirectoryAccessRule`\.
+ `FullControl` role added to SQL Server Service user as part of `FileSystemRights`\.

### Self\-managed Active Directory<a name="launch-wizard-ad-onprem"></a>

If you are [deploying SQL Server into an existing VPC across multiple Availability Zones and connecting to a self-managed Active Directory](launch-wizard-deployment-options.md#option-4) or [deploying SQL Server into an existing VPC on a single node and connecting to a self-managed Active Directory](launch-wizard-deployment-options.md#option-5), verify the following prerequisites\.
+ If your self\-managed Active Directory resides in another network than where you are deploying SQL Server, make sure you have connectivity between your VPC and the self\-managed Active Directory network\. You must also be able to connect to any DNS servers you specify during deployment from your VPC\. For more information, see [Network\-to\-Amazon VPC connectivity options](https://docs.aws.amazon.com/whitepapers/latest/aws-vpc-connectivity-options/network-to-amazon-vpc-connectivity-options.html)\.
+ Your SQL Server resources must be able to perform DNS resolution from within the VPC to any DNS servers you specify\. For options on how to set this up, see [ How to Set Up DNS Resolution Between On\-Premises Networks and AWS Using AWS Directory Service and Amazon Route 53](https://aws.amazon.com/blogs/security/how-to-set-up-dns-resolution-between-on-premises-networks-and-aws-using-aws-directory-service-and-amazon-route-53/) or [How to Set Up DNS Resolution Between On\-Premises Networks and AWS Using AWS Directory Service and Microsoft Active Directory](https://aws.amazon.com/blogs/security/how-to-set-up-dns-resolution-between-on-premises-networks-and-aws-using-aws-directory-service-and-microsoft-active-directory/)\.
+ The domain functional level of your Active Directory domain controller must be Windows Server 2012 or later\.
+ The firewall on the Active Directory domain controllers should allow the connections from the Amazon VPC from which you will create the Launch Wizard deployment\. At a minimum, your configuration should include the ports mentioned in [How to configure a firewall for Active Directory domains and trusts](https://support.microsoft.com/en-us/help/179442/how-to-configure-a-firewall-for-domains-and-trusts)\.
+ The domain user requires the following permissions in the [Active Directory Default organizational unit \(OU\)](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/creating-an-organizational-unit-design) to enable Launch Wizard to perform the deployment successfully:
  + `Reset password`
  + `Write userAccountControl`
  + `Create user accounts`
  + `Create computer objects`
  + `Read all properties`
  + `Modify permissions`

## AWS Identity and Access Management \(IAM\)<a name="launch-wizard-iam"></a>

The following steps to establish the AWS Identity and Access Management \(IAM\) role and set up the IAM user for permissions are typically performed by an IAM administrator for your organization\. 

### One\-time creation of IAM Role<a name="launch-wizard-iam-role"></a>

On the **Choose Application** page of Launch Wizard, under **Permissions**, Launch Wizard displays the IAM role required for the Amazon EC2 instances created by Launch Wizard to access other AWS services on your behalf\. When you select **Next**, Launch Wizard attempts to discover the IAM role in your account\. If the role exists, it is attached to the instance profile for the EC2 instances that Launch Wizard will launch into your account\. If the role does not exist, Launch Wizard attempts to create the role with the same name, `AmazonEC2RoleForLaunchWizard`\. This role is comprised of two IAM managed policies: `AmazonSSMManagedInstanceCore` and `AmazonEC2RolePolicyForLaunchWizard`\. After the role is created, the IAM administrator can delegate the application deployment process to another IAM user who, in turn, must have the Launch Wizard IAM managed policy described in the following section\.

### IAM user setup<a name="launch-wizard-iam-user-setup"></a>

To deploy a SQL Server Always On application with Launch Wizard, you must create an [Identity and Access Management \(IAM\) policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) and attach it to your IAM user identity\. The IAM policy defines the user permissions\. If you do not already have an IAM user in your account, follow the steps listed in [Create an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)\.

When you have an IAM user in your account, create an IAM policy\.

1. Go to the IAM console at [ https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam)\. In the left navigation pane, choose **Policies**\.

1. Choose **Users** from the left navigation pane\. 

1. Select the **User name** of the user to which you want to attach the policy\.

1. Select **Add permissions**\. 

1. Select **Attach existing policies directly**\. 

1. Search for the policy named **AmazonLaunchWizard\_Fullaccess** and select the check box to the left of the policy name\.

1. Select **Next: Review**\. 

1. Verify that the correct policy is listed, and then select **Add permissions**\. 

**Important**  
Log in with the user associated with the above policy when you use Launch Wizard\. 

## AWS Secrets Manager<a name="launch-wizard-sql-prerequisites-secrets-manager"></a>

Launch Wizard uses AWS Secrets Manager to manage your domain and SQL Server account passwords\. Your username and password is stored in Secrets Manager and is retrieved during the build process\. The following resource policy is added to the secret so that the Launch Wizard IAM role can retrieve the secret\. For more information about Secrets Manager, see the [AWS Secrets Manager User Guide](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)\.

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
    } ]
  }
```

## Requirements for using custom AMIs \(deployment on Windows\)<a name="launch-wizard-custom-ami"></a>

We recommend that you use Amazon Windows license\-included AMIs whenever possible\. There are scenarios for which you may want to use a custom Windows AMI\. For example, you may have existing licenses \(BYOL\), or you may have made changes to one of our public images and re\-imaged it\.

If you use Amazon Windows license\-included AMIs, you are not required to perform any pre\-checks on the AMI to ensure that it meets Launch Wizard requirements\.

Launch Wizard relies on user data to begin the process of configuring SQL Server or RGW instances to launch in your account\. For more information, see [User Data Scripts](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-windows-user-data.html)\. By default, all AWS Windows AMIs have user data execution enabled for the initial launch\. To ensure that your custom AMIs are set up to run the User Data script at launch, follow the AWS recommended method to prepare your AMIs using [EC2Launch v2](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2launch-v2.html)\. For more information about how to prepare your custom AMI using the options to `Shutdown with Sysprep` or `Shutdown without Sysprep`, see [Create a Standard Amazon Machine Image Using Sysprep](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/Creating_EBSbacked_WinAMI.html#ami-create-standard) or [EC2Launch v2 and Sysprep](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2launch.html#ec2launch-v2-sysprep)\. If you want to directly enable user data as part of the custom AMI creation process, follow the steps for `Subsequent Reboots` or `Starts` under [Running Commands on Your Windows Instance at Launch](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-windows-user-data.html)\. 

If you use a custom Windows AMI, the volume drive letter for the root partition should be `C:` because EC2Launch v2 and EC2Config rely on this configuration to install the components\. 

While not exhaustive, the following requirements cover most of the configurations whose alteration might impact the successful deployment of a SQL Server Always On application using Launch Wizard\.


**Support matrix**  

| SQL Server Version | Windows Server 2012 R2 | Windows Server 2016 | Windows Server 2019 | 
| --- | --- | --- | --- | 
|  SQL Server 2016  |  YES  |  YES  |  YES  | 
|  SQL Server 2017  |  YES  |  YES  |  YES  | 
| SQL Server 2019 | Currently not supported\.  | YES | YES | 

**OS and SQL requirements**
+ Microsoft Windows Server 2012 R2 \(Datacenter\) \(64\-bit only\)
+ Microsoft Windows Server 2016 \(Datacenter\) \(64\-bit only\)
+ Microsoft Windows Server 2019 \(Datacenter\) \(64\-bit only\)
+ MBR\-partitioned volumes and GUID Partition Table \(GPT\) partitioned volumes that are formatted using the NTFS file system
+ English language pack only
+ SQL Server Enterprise Edition 2017/2016 or Standard Edition 2017/2016
+ SQL Server Enterprise Edition 2019 or Standard Edition 2019
+ The root volume drive for the custom AMI should be `C:`
+ SQL Server is installed on the root drive

**AWS software and drivers**
+ EC2Launch v2 \([supported AMIs](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2launch-v2-install.htm)\)
+ EC2Config service \(Windows Server 2012 R2\)
+ EC2Launch \(Windows Server 2016\)
+ AWS SSM \([SSM agent must be installed](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-install-win.html)\)
+ AWS Tools for Windows PowerShell
+ Network drivers \(SRIOV, ENA\)
+ Storage drivers \(NVMe, AWS PV\)

## Requirements for using custom AMIs \(deployment on Linux\)<a name="launch-wizard-custom-ami-linux"></a>

There are occasions when you may want to use a custom Linux AMI\. For example, you may have existing licenses \(BYOL\), or you may have made changes to one of our public images and re\-imaged it\.

If you use a custom Linux AMI, you must adhere to the following requirements:
+ The operating system must be Ubuntu version 18\.04 LTS\.
+ The system installer and administrator must be a sudo user and be able to log in to the cluster nodes using SSH\.
+ SQL Server for Linux must be a default installation\.
+ The SQL Server for Linux version must be 2019\.
+ The latest Microsoft SQL tools must be installed\.

## Requirements for using Windows license\-included AMIs \(SQL Server FCI configuration only\)<a name="launch-wizard-sql-prerequisites-license-included-ami"></a>

You can use Windows license\-included \(LI\) AMIs with SQL Bring\-Your\-Own\-License \(BYOL\)\. Your SQL media must meet the following requirements to use Windows LI AMIs with SQL BYOL\. Linux AMIs are not supported\.

The SQL media must be:
+ An ISO file\.
+ Hosted in an Amazon S3 bucket prefixed with `LaunchWizard-*`\.
+ Included in a folder within the Amazon S3 bucket\.
+ Included in a public folder so that Launch Wizard can download and install the media\.

## Requirements for using Amazon FSx<a name="launch-wizard-sql-prerequisites-fsx"></a>

Launch Wizard uses continuously available Amazon FSx file shares to host clustered databases\. The Amazon FSx file shares are accessible from within an instance joined to the domain\. You can either create a new Active Directory or connect to an existing Active Directory \(managed or self\-managed\)\. If you connect to an existing Active Directory, you can use preexisting security groups \. The security groups must satisfy port and security requirements for FSx to communicate with the domain, as described in [Using Amazon FSx with your self\-managed Microsoft Active Directory](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/self-managed-AD.html) and [Using Amazon FSx with AWS Directory Service for Microsoft Active Directory](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/fsx-aws-managed-ad.html)\.

If you are using an existing AWS Managed Active Directory instance, you must specify the ID of the managed Active Directory instance for FSx to be able to join the domain\. The account must have the same access rights in the domain as described in [Using Amazon FSx with your self\-managed Microsoft Active Directory](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/self-managed-AD.html) and [Using Amazon FSx with AWS Directory Service for Microsoft Active Directory](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/fsx-aws-managed-ad.html)\.

For Amazon FSx using NetApp ONTAP, Launch Wizard creates security groups in order to access the ONTAP file system and to set up failover clustering\. For port requirements, see [File System Access Control with Amazon VPC](https://docs.aws.amazon.com/fsx/latest/ONTAPGuide/limit-access-security-groups.html)\.

**Backup schedule**  
Launch Wizard uses FSx defaults for setting up the backup schedule\. You can change the default settings in the FSx console after the build completes\.

The `WeeklyMaintenanceStartime` follows the format `day of the week:time`, where Monday is indicated by `1`\. The maintenance start time is set to begin on Saturday at 10pm\.

```
WeeklyMaintenanceStartTime: '6:22:00'
DailyAutomaticBackupStartTime: '01:00'
AutomaticBackupRetentionDays: 7
```

**Amazon FSx using NetApp ONTAP**  
Amazon FSx using NetApp ONTAP creates a new ONTAP file system for use with your Launch Wizard SQL deployment\. We use the formulas in the following table to calculate volume and LUN storage for optimal performance\.

These values can be modified post deployment\.


| Storage type | Size in GB | Sizing calculations | 
| --- | --- | --- | 
|  FSx storage  |  1024  | Size in GB | 
|  Volume storage  |  870\.4  | 85% of total storage FSx capacity | 
|  LUN storage  |  696\.32  | 80% of volume storage \(65% of total FSx storage\) | 
| SQL data LUN size | 522\.24 | 60% of LUN storage | 
| SQL log LUN size | 139\.264 | 20% of SQL Data LUN size | 

**Backup schedule for ONTAP**  
By default, ONTAP backups are disabled during builds\. You can set your own backup schedule from the Amazon FSx console\. Choose the **Backup** tab\. Then, choose **Update** to update the backup settings\. 

**Note**  
When you delete a Launch Wizard deployment that uses ONTAP, FSx creates a backup of the ONTAP volume before deleting the file system\. You can delete the backup from the Amazon FSx console if it is not required\. For more information, see [Deleting backups](https://docs.aws.amazon.com/fsx/latest/ONTAPGuide/using-backups.html#delete-backups) in the *FSx for ONTAP User Guide*\.

## Configuration settings \(deployment on Windows\)<a name="launch-wizard-config"></a>

The following configuration settings are applied when deploying a SQL Server Always On application with Launch Wizard\.


| Setting | Applies to | 
| --- | --- | 
|  Current EC2Launch v2 and SSM Agent  |  [Supported AMIs](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2launch-v2-install.htm)  | 
|  Current EC2Config and SSM Agent  |  Windows Server 2012 R2  | 
|  Current EC2Launch and SSM Agent  |  Windows Server 2019 and 2016  | 
|  Current AWS PV, ENA, and NVMe drivers  |  Windows Server 2019, 2016, and 2012 R2  | 
|  Current SRIOV drivers  |  Windows Server 2019, 2016, and 2012 R2  | 
|  Microsoft SQL Server: Latest service pack SQL Service configured to start automatically SQL Service running `BUILTIN\Administrators` added to the `SysAdmin` server role TCP port `1433` and UDP port `1434` open  |  Windows Server 2019, 2016, and 2012 R2  | 
|  Allow ICMP traffic through the firewall  |  Windows Server 2019, 2016, and 2012 R2  | 
|  Allow RDP traffic through host firewall  |  Windows Server 2019, 2016, and 2012 R2  | 
|  Enable file and printer sharing  |  Windows Server 2012 R2  | 
|  `RealTimeIsUniversal` registry key set  |  Windows Server 2019, 2016, and 2012 R2  | 
| SQL Server FCI | Windows Server 2019 and 2016 SQL Server 2019, 2017, and 2016 | The following AMI settings can impact the Launch Wizard deployment:

**System Time**  
**RealTimeIsUniversal**\. If disabled, Windows system time drifts when the time zone is set to a value other than UTC\.

**Windows Firewall**  
In most cases, Launch Wizard configures the correct protocols and ports\. However, custom Windows Firewall rules could impact the cluster service\. To ensure that your custom AMI works with Launch Wizard, see [Service overview and network port requirements for Windows](https://support.microsoft.com/en-us/help/832017/service-overview-and-network-port-requirements-for-windows)\.

**Remote Desktop**  
**Service Start**\. Remote Desktop service must be enabled\.  
**Remote Desktop Connections**\. Must be enabled\.

**EC2Config** \(Server 2012 R2\)  
**Installation**\. We recommend using the latest version of EC2Config\.  
**Service Start**\. EC2Config service should be enabled\.

**Network Interface**  
**DHCP Service Startup**\. DHCP service should be enabled\.  
**DHCP on Ethernet**\. DHCP should be enabled\.

**Microsoft SQL Server**  
**TCPIP**\. Must be enabled for protocols in SQL Configuration Manager\.

**PowerShell**  
**Execution Policy**\. The execution policy in all AWS license\-included AMIs is set to `Unrestricted`\. We recommend that you set this policy to `Unrestricted` when you set up SQL Server Always On Availability Groups using Launch Wizard\. You can change the policy when setup is complete\. 