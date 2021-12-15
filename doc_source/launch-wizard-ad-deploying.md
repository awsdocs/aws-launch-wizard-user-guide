# Deploy an application with AWS Launch Wizard for Active Directory<a name="launch-wizard-ad-deploying"></a>

The following steps guide you through a domain controller deployment with AWS Launch Wizard after you have launched it from the console\.

1. When you select **Create deployment** from the AWS Launch Wizard landing page, you are directed to the **Choose application** page where you are prompted to select the type of application that you want to deploy\. Select **Active Directory** and choose **Create deployment**\.

1. After you select the application to configure for deployment, under **Permissions**, Launch Wizard displays the AWS Identity and Access Management \(IAM\) role required for Launch Wizard to access other AWS services on your behalf\. For more information about setting up the IAM role for Launch Wizard, see [AWS Identity and Access Management \(IAM\)](launch-wizard-ad-setting-up.md#launch-wizard-ad-iam)\. Choose **Next** \.

1. On the **Configure application settings** page, enter specifications for the new deployment\. The following tabs provide information about the specification fields\.

------
#### [ General settings and Active Directory \(AD\) installation ]

**General settings**
   + **Deployment name**\. Enter a unique application name for your deployment\.
   + **Simple Notification Service \(SNS\) topic ARN \(Optional\)**\. Specify an Amazon SNS topic where AWS Launch Wizard can send notifications and alerts\. For more information, see the [https://docs.aws.amazon.com/sns/latest/dg/welcome.html](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\.
   + **Enable rollback on failed deployment**\. By default, if a deployment fails, your provisioned resources will not be rolled back/deleted\. This default configuration helps you to troubleshoot errors at the resource level as you debug deployment issues\. If you want your provisioned resources to be immediately deleted if a deployment fails, select the check box\.

**Installation type**  
Choose whether you want to deploy Active Directory on Amazon EC2 instances or on premises\. When you deploy Active Directory on Amazon EC2, it is deployed in a VPC configured for high availability\.

   For the **Domain settings** that follow, choose the tab that applies to the AMI type you plan to use for the deployment: **Domain settings — license\-included AMI** or **Domain settings — custom AMI**\.

------
#### [ Domain settings – license\-included AMI ]

   Enter the following specifications to configure your domain controllers on a license\-include AMI\.

**Number of domain controllers**  
Select the number of servers on which you want to install Active Directory\.

**Domain controller details — optional**  
For each domain controller, enter a name\.

**AMI installation type**

   Choose to use an AWS license\-included AMI with Windows installed\.
   + **License\-included AMI**\. Choose the license\-included AMI you want to use to launch your domain controller environment\.
   + **Active Directory \(AD\) settings**\. Choose whether you want to add domain controllers to an existing Active Directory or you want to create a new Active Directory\.

**Add domain controllers to an existing Active Directory**
     + **Active Directory DNS domain name**\. Enter the DNS domain name for your directory to connect to an existing Active Directory\. For more information about DNS support of Active Directory, see the [Microsoft Active Directory](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc759186(v=ws.10)#dns-support-for-activedirectory-1) documentation\.
     + **Domain NetBIOS name\.** The NetBIOS name is the subdomain of the DNS domain name\. Enter the NetBIOS name to connect to Active Directory\.
     + **Domain Administrator secret name\.** Enter the name of the AWS Secrets Manager name of the Domain Administrator to access stored credentials\.
     + **Add permissions to AWS Secrets Manager — required\.** Follow the instructions to add the required resource policy for Launch Wizard to access Secrets Manager\.
     + **Domain DNS IP address for resolution\.** Enter the IP address of the DNS server to which you are connecting\.

**Create a new Active Directory**
     + **Active Directory DNS domain name**\. Enter a DNS domain name for your new Active Directory\. For more information about DNS support of Active Directory, see the [Microsoft Active Directory](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc759186(v=ws.10)#dns-support-for-activedirectory-1) documentation\.
     + **Domain NetBIOS name\.** The NetBIOS name is the sub\-domain of the DNS domain name\. Enter a NetBIOS name to connect to Active Directory\.
     + **Domain Administrator username\.** Launch Wizard applies the `Administrator` username by default\.
     + **Additional settings — optional\.** 
       + **AD Certificate Services \(AD CS\) installation\.** Select whether you want to install Active Directory Certificate Services\. For more information about AD CS, see the [Microsoft documentation](https://social.technet.microsoft.com/wiki/contents/articles/1137.active-directory-certificate-services-ad-cs-introduction.aspx)\.
       + **ADCS Certificate Server name \(if selected\)**\. If you selected to install AD CS, enter the AD CS certificate server name to use\.
       + **Key length**\. Select the key length of the encryption key\.
       + **Key hash algorithm**\. Select the algorithm used for key encryption\.
       + **Key validity**\. Select a key validity period in years\. The key validity period determines the expiration date of the certificates\.
       + **Forest trust**\. Select whether you want to create a forest trust with another forest\. A forest trust is a transitive trust between two forests that allows users in any domain in one forest to be authenticated by any domain in another forest\. For more information, see the [Microsoft Active Directory documentation](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/concepts-forest-trust#forest-trusts)\.
         + **Fully qualified domain name of forest**\. If you selected to create a forest trust, enter the fully qualified domain name of the forest\. 
         + **Direction of trust**\. Select the direction of the trust\. Trusts can be one\-way or two\-way\.
         + **Conditional forwarder IP address**\. Enter the IP address of the conditional forwarder that is set up in your forest\.

------
#### [ Domain settings – custom AMI ]

   Enter the following specifications to configure your domain controllers on a custom AMI\.

**Number of domain controllers**  
Select the number of servers on which you want to install Active Directory\.

**Domain controller details — optional**  
For each domain controller, enter a name\.

**AMI installation type**

   Choose to use a custom AMI\. Verify that the AMI is configured to meet all of the required installation parameters described in [Requirements for using custom AMIs](launch-wizard-ad-setting-up.md#launch-wizard-ad-custom-ami)\.
   + **Custom AMI ID**\. Select the custom AMI you want to use to launch your domain controller environment\.
   + **Active Directory DNS domain name**\. Enter the DNS domain name for your directory to connect to an existing Active Directory\. For more information about DNS support of Active Directory, see the [Microsoft Active Directory](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc759186(v=ws.10)#dns-support-for-activedirectory-1) documentation\.
   + **Domain NetBIOS name\.** The NetBIOS name is the subdomain of the DNS domain name\. Enter the NetBIOS name to connect to Active Directory\.
   + **Domain Administrator secret name\.** Enter the name of the AWS Secrets Manager name of the Domain Administrator to access stored credentials\.
   + **Add permissions to AWS Secrets Manager — required\.** Follow the instructions to add the required resource policy for Launch Wizard to access AWS Secrets Manager\.
   + **Domain DNS IP address for resolution\.** Enter the IP address of the DNS server to which you are connecting\.

------
#### [ Connectivity ]

   Enter specifications for how you want to connect to your application instance and what kind of virtual private cloud \(VPC\) you want to set up\. 

   **Key pair name**
   + Select an existing key pair from the dropdown list or create a new one\. If you select **Create new key pair name** to create a new key pair, you are directed to the Amazon EC2 console\. From there, under **Network and Security**, choose **Key Pairs**\. Choose **Create a new key pair**, enter a name for the key pair, and then choose **Download Key Pair**\.
**Important**  
This is the only chance for you to save the private key file, so be sure to download it and save it in a safe place\. You must provide the name of your key pair when you launch an instance, and provide the corresponding private key each time that you connect to the instance\. 

     Return to the Launch Wizard console and choose the refresh button next to the **Key Pairs** dropdown list\. The newly created key pair appears in the dropdown list\. For more information about key pairs, see [Amazon EC2 Key Pairs and Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\.

   **Virtual Private Cloud \(VPC\)**\. Choose whether you want to use an existing VPC or create a new VPC\.

**Select Virtual Private Cloud \(VPC\)**
   + **Select Virtual Private Cloud \(VPC\)** option\. Choose the VPC that you want to use from the dropdown list\. Your VPC must contain one public subnet and, at least, two private subnets\. Your VPC must be associated with a [DHCP Options Set](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html) to enable DNS translations to work\. The private subnets must have outbound connectivity to the internet and other AWS services \(Amazon S3, AWS CloudFormation, SSM, CloudWatch Logs\)\. We recommend that you enable this connectivity with a NAT Gateway\. For more information about NAT Gateways, see [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) in the Amazon VPC User Guide\.
     + For deployment of a new Active Directory on Amazon EC2, choose your **Remote Desktop Gateway \(RDG\) preferences**\. This option is not available when you add domain controllers to an existing infrastructure\. 
       + Choose whether you want to set up Remote Desktop Gateway now, or set it up later\.
         + If you choose to set up RDG now, enter the **Remote Desktop Gateway CIDR**\. Select **Custom IP** from the dropdown list and enter the CIDR block\. If you do not specify any value for the Custom IP parameter, Launch Wizard does not set the inbound RDP access \(Port 3389\) from any IP\. You can choose to do this later by modifying the security group settings by using the Amazon EC2 console\. See [Adding a Rule for Inbound RDP Traffic to a Windows Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/authorizing-access-to-an-instance.html#add-rule-authorize-access) for instructions on adding a rule that allows inbound RDP traffic to your RDGW instance\. 
     + **Availability Zone configuration**\. You must choose at least two Availability Zones, with one private subnet for each zone that you select\. From the dropdown lists, select the **Availability Zones** within which you want to deploy the domain controllers\. Depending on the number of domain controllers you plan to deploy, you may have to specify a ** private subnet** for each of them\. Cross\-Region replication is not supported\. 

***To create a private subnet***

       If a subnet doesn't have a route to an internet gateway, the subnet is known as a private subnet\. To create a private subnet, you can use the following steps\. We recommend that you enable the outbound connectivity for each of your selected private subnets using a NAT Gateway\. To enable outbound connectivity from private subnets with public subnet, see the steps in [Creating a NAT Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-creating) to create a NAT Gateway in your chosen public subnet\. Then, follow the steps in [Updating Your Route Table ](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-create-route)for each of your chosen private subnets\.
       + Follow the steps in [Creating a Subnet](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#AddaSubnet) in the Amazon VPC User Guide using the existing VPC you will use in AWS Launch Wizard\. 
       + When you create a VPC, it includes a main route table by default\. On the **Route Tables** page in the Amazon VPC console, you can view the main route table for a VPC by looking for **Yes** in the **Main** column\. The main route table controls the routing for all subnets that are not explicitly associated with any other route table\. If the main route table for your VPC has an outbound route to an internet gateway, then any subnet created using the previous step, by default, becomes a public subnet\. To ensure the subnets are private, you may need to create separate route tables for your private subnets\. These route tables must not contain any routes to an internet gateway\. Alternatively, you can create a custom route table for your public subnet and remove the internet gateway entry from the main route table\.
     + **Outbound connectivity**\. Confirm that you have set up a public subnet and that each of the private subnets are configured to send traffic\.

**Create a new virtual private cloud \(VPC\)**

   AWS Launch Wizard creates your VPC\. You can optionally enter a VPC name tag\.
   + **Remote Desktop Gateway CIDR**\. Select **Custom IP** from the dropdown list\. Enter the CIDR block\. If you do not specify any value for the Custom IP parameter, Launch Wizard does not set the inbound RDP access \(Port 3389\) from any IP\. You can choose to do this later by modifying the security group settings via the Amazon EC2 Console\. See [Adding a Rule for Inbound RDP Traffic to a Windows Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/authorizing-access-to-an-instance.html#add-rule-authorize-access) for instructions on adding a rule that allows inbound RDP traffic to your RDGW instance\. 

------

1. When you are satisfied with your configuration selections, select **Next**\. If you don't want to complete the configuration, select **Cancel**\. When you select **Cancel**, all of the selections on the settings page are lost and you are returned to the landing page\. To go to the previous screen, select **Previous**\.

1. After configuring your application, you are prompted to define the infrastructure requirements for the new deployment on the **Define infrastructure requirements** page\. 

**Storage and Compute**

   Enter your instance size preferences and view recommended resources\.
   + **Instance sizing** Choose whether you want to base the size of your deployed instances on infrastructure requirements or you want to select the instance type\. If you choose to select the infrastructure requirements, enter the number of Active Directory users in the environment\. If you choose to select the instance type, select the instance type from the dropdown\.
   + **Recommended resources**\. Launch Wizard displays the system\-recommended resources based on your infrastructure selections\. If you want to change the recommended resources, select different infrastructure requirements\. 

1. When you are satisfied with your infrastructure selections, choose **Next**\. If you don't want to complete the configuration, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. To go to the previous screen, select **Previous**\.

1. On the **Review** page, review your configuration details\. If you want to make changes, select **Edit** or **Previous**\. To stop, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. If you are ready to deploy, read and select the check box next to the **Acknowledgment**\. Then choose **Deploy**\.

1. Launch Wizard validates the inputs and notifies you if something must be addressed\. 

1. When validation is complete, Launch Wizard deploys your AWS resources and configures your domain controllers\. Launch Wizard provides you with status updates about the progress of the deployment on the **Deployments** page\. From the **Deployments** page, you can also view the list of current and previous deployments\.

1. When your deployment is ready, you see notification that your domain controllers are successfully deployed\. If you have set up SNS notification, you are also alerted through Amazon SNS\. You can manage and access all of the resources related to your domain controllers by selecting **Manage**\.

1. When the domain controllers are deployed, you can access your Amazon EC2 instances through the Amazon EC2 console\. You can also use [AWS SSM](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html) to manage your domain controllers for future updates and patches through built\-in integration through resource groups\.