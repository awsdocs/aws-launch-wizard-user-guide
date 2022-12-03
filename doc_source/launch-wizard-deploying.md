# Deploy an application with AWS Launch Wizard for SQL Server on Windows<a name="launch-wizard-deploying"></a>

## Access AWS Launch Wizard<a name="accessing-launch-wizard"></a>

You can launch AWS Launch Wizard from the [AWS Launch Wizard console](https://console.aws.amazon.com/launchwizard)\.

## Deploy AWS Launch Wizard on Windows<a name="deploy-console-launch-wizard"></a>

### Deploy SQL Server Always On application<a name="deploy-console-launch-wizard-always-on"></a>

The following steps guide you through a SQL Server Always On application deployment with AWS Launch Wizard after you have launched it from the console\.

1. When you select **Choose application** from the AWS Launch Wizard landing page, you are directed to the **Choose application** wizard, where you are prompted to select the type of application that you want to deploy\. Select **Microsoft SQL Server**, then **Create deployment**\.

1. Under ** Review Permissions**, Launch Wizard displays the AWS Identity and Access Management \(IAM\) role required for Launch Wizard to access other AWS services on your behalf\. For more information about setting up IAM for Launch Wizard, see [AWS Identity and Access Management \(IAM\)](launch-wizard-setting-up.md#launch-wizard-iam)\. Choose **Next** \.

1. On the **Configure application settings** page, select the **Operating System** on which you want to install SQL Server — in this case, **Windows**\.

1. **Deployment model**\. Choose **High availability deployment** to deploy your SQL Server Always On application across multiple Availability Zones or **Single instance deployment** to deploy your SQL Server application on a single node\.

1. You are prompted to enter the specifications for the new deployment\. The following tabs provide information about the specification fields\.

------
#### [ General ]
   + **Deployment name**\. Enter a unique application name for your deployment\.
   + **Simple Notification Service \(SNS\) topic ARN — optional**\. Specify an SNS topic where AWS Launch Wizard can send notifications and alerts\. For more information, see the [https://docs.aws.amazon.com/sns/latest/dg/welcome.html](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\.
   + **CloudWatch application monitoring \(optional for HA deployments\)**\. Select the check box to set up monitors and automated insights for your deployment using CloudWatch Application Insights\. For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch-application-insights.html)\.
   + **Enable rollback on failed deployment**\. By default, if a deployment fails, your provisioned resources will not be rolled back/deleted\. This default configuration helps you to troubleshoot errors at the resource level as you debug deployment issues\. If you want your provisioned resources to be immediately deleted if a deployment fails, select the check box\.

------
#### [ Connectivity ]

   Enter specifications for how you want to connect to your instance and configure your Virtual Private Cloud \(VPC\)\. 

   **Key pair name**
   + Select an existing key pair from the dropdown list or create a new one\. If you select **Create new key pair name**, you are directed to the Amazon EC2 console\. From there, under **Network and Security**, choose **Key Pairs**\. Choose **Create a new key pair**, enter a name for the key pair, and then choose **Download Key Pair**\.
**Important**  
This is the only opportunity for you to save the private key file\. Download it and save it in a safe place\. You must provide the name of your key pair when you launch an instance and provide the corresponding private key each time that you connect to the instance\. 

     Return to the Launch Wizard console and choose the refresh button next to the **Key Pairs** dropdown list\. The newly created key pair appears in the dropdown list\. For more information about key pairs, see [Amazon EC2 Key Pairs and Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\.

   **Tenancy model \(HA deployments only\)**

   Select your preferred tenancy\. Each instance that you launch into a VPC has a tenancy attribute\. The **Shared** tenancy option means that the instance runs on shared hardware\. The **Dedicated Host \(HA deployments\)** tenancy option means that the instance runs on a Dedicated Host, which is an isolated server with configurations that you can control\. For more information, see [Dedicated Hosts](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/dedicated-hosts-overview.html)\. 

   **Virtual Private Cloud \(VPC\)**\. Choose whether you want to use an existing VPC or create a new VPC\.
   + **Select Virtual Private Cloud \(VPC\)** option\. Choose the VPC that you want to use from the dropdown list\. If you choose to enable Remote Desktop Gateway access on single\-node deployments, then your VPC must include one public subnet and one private subnet\. It must include at least two private subnets for HA deployments \. Your VPC must be associated with a [DHCP Options Set](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html) to enable DNS translations to work\. The private subnets must have outbound connectivity to the internet and other AWS services \(S3, CFN, SSM, Logs\)\. We recommend that you enable this connectivity with a NAT Gateway\. For more information about NAT Gateways, see [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) in the Amazon VPC User Guide\.
     + **Public Subnet**\. If you choose to enable Remote Desktop Gateway access on single\-node deployments, then your VPC must include one public subnet and one private subnet\. It must include at least two private subnets for HA deployments\. Choose a public subnet for your VPC from the dropdown list\. To continue, you must select the check box that indicates that the public subnet has been set up and the private subnets have outbound connectivity enabled\. 

**To add a new public subnet**

       If a subnet's traffic is routed to an internet gateway, the subnet is known as a public subnet\. If, however, a subnet doesn't have a route to the internet gateway, the subnet is known as a private subnet\. To use an existing VPC that does not have a public subnet, you can add a new public subnet using the following steps\.
       + Follow the steps in [Creating a Subnet in the Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Create_Subnet) using the existing VPC you intend to use AWS Launch Wizard\.
       + To add an internet gateway to your VPC, follow the steps in [Attaching an Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Attach_Gateway) in the Amazon VPC User Guide\.
       + To configure your subnets to route internet traffic through the internet gateway, follow the steps in [Creating a Custom Route Table](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Routing) in the Amazon VPC User Guide\. Use IPv4 format \(0\.0\.0\.0/0\) for Destination\.
       + The public subnet should have the “auto\-assign public IPv4 address” setting enabled\. To enable this setting, follow the steps in [Modifying the Public IPv4 Addressing Attribute for Your Subnet](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-ip-addressing.html#subnet-public-ip) in the Amazon VPC User Guide\.
     + **Availability Zone \(AZ\) configuration**\. You must choose at least two Availability Zones for High Availability \(HA\) deployments and one Availability Zone for single\-node deployments, with one private subnet for each zone that you select\. For HA deployments, select the **Availability Zones** within which you want to deploy your **primary** and **secondary** SQL nodes\. Depending on the number of secondary nodes that you plan to use to set up a SQL Server Always On deployment, you may have to specify a ** private subnet** for each of them\. Cross\-Region replication is not supported\. 

**To create a private subnet**

       If a subnet doesn't have a route to an internet gateway, the subnet is known as a private subnet\. To create a private subnet, you can use the following steps\. We recommend that you enable the outbound connectivity for each of your selected private subnets using a NAT Gateway\. To enable outbound connectivity from private subnets with public subnet, see the steps in [Creating a NAT Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-creating) to create a NAT Gateway in your chosen public subnet\. Then, follow the steps in [Updating Your Route Table ](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-create-route)for each of your chosen private subnets\.
       + Follow the steps in [Creating a Subnet](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#AddaSubnet) in the Amazon VPC User Guide using the existing VPC you will use in AWS Launch Wizard\. 
       + When you create a VPC, it includes a main route table by default\. On the **Route Tables** page in the Amazon VPC console, you can view the main route table for a VPC by looking for Yes in the Main column\. The main route table controls the routing for all subnets that are not explicitly associated with any other route table\. If the main route table for your VPC has an outbound route to an internet gateway, then any subnet created using the previous step, by default, becomes a public subnet\. To ensure the subnets are private, you may need to create separate route table\(s\) for your private subnets\. These route tables must not contain any routes to an internet gateway\. Alternatively, you can create a custom route table for your public subnet and remove the internet gateway entry from the main route table\.

       If you selected **Dedicated host** tenancy, you must select a Dedicated Host for each Availability Zone\. If you have not allocated any dedicated hosts to your account, you can choose **Create new dedicated host** to do so from the EC2 console\.
     + **Remote Desktop Gateway preferences \(single\-node deployments only\)**\. When you select **Set up Remote Desktop Gateway**, enter the public subnet into which to deploy the RDGW instance\.
     + **Remote Desktop Gateway access — Optional**\. Select **Custom IP** from the dropdown list\. Enter the CIDR block\. If you do not specify any value for the Custom IP parameter, Launch Wizard does not set the inbound RDP access \(Port 3389\) from any IP\. You can choose to do this later by modifying the security group settings via the Amazon EC2 console\. See [Adding a Rule for Inbound RDP Traffic to a Windows Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html#add-rule-authorize-access) for instructions on adding a rule that allows inbound RDP traffic to your RDGW instance\. 
   + **Create new Virtual Private Cloud \(VPC\)** option\. Launch Wizard creates your VPC\. You can optionally enter a **VPC name tag**\. If you selected **Dedicated Host** tenancy for high availability deployments, select a primary and secondary Dedicated Host\. If you haven't allocated any Dedicated Hosts to your account, select **Create a new dedicated host**\. You will be directed to the EC2 console to create the new host\.
     + **Remote Desktop Gateway preferences \(single\-node deployments only\)**\. When you select **Set up Remote Desktop Gateway**, only the Remote Desktop Gateway access information will be taken from the VPC\.
     + **Remote Desktop Gateway access — Optional**\. Select **Custom IP** from the dropdown list\. Enter the CIDR block\. If you do not specify any value for the Custom IP parameter, Launch Wizard does not set the inbound RDP access \(Port 3389\) from any IP\. You can choose to do this later by modifying the security group settings via the Amazon EC2 Console\. See [Adding a Rule for Inbound RDP Traffic to a Windows Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html#add-rule-authorize-access) for instructions on adding a rule that allows inbound RDP traffic to your RDGW instance\. 

------
#### [ Active Directory ]

   You can connect to an existing Active Directory or, for high availability deployments, you can create a new one\. If you selected the **Create new Virtual Private Cloud \(VPC\)** option for high availability deployments, you must select **Create a new Active Directory**\.

**Connecting to existing AWS Managed Active Directory or self\-managed Active Directory**

   From the dropdown list, select whether you want to use **AWS Managed Active Directory**, or **Self\-managed Active Directory**\. If you select **Self\-managed Active Directory**, select the check box to verify that you have ensured a connection between the Active Directory and the VPC\.

   Follow the steps for granting permissions in the Active Directory Default Organizational Unit \(OU\)\. 
   + **Domain user name and password**\. Enter the user name and password for your directory\. For required permissions for the domain user, see [Active Directory \(Windows deployment\)Active Directory \(Windows\)](launch-wizard-setting-up.md#launch-wizard-ad)\. Launch Wizard stores the password in AWS Secrets Manager as a secure string parameter\. It does not store the password on the service side\. To create a functional SQL Server Always On deployment, it reads from AWS Secrets Manager\.
   + **DNS address**\. Enter the IP address of the DNS servers to which you are connecting\. These servers must be reachable from within the VPC that you selected\. 
   + **Optional DNS address**\. If you would like to use a backup DNS server, enter the IP address of the DNS server that you want to use as backup\. These servers must be reachable from within the VPC that you selected\. 
   + **Domain DNS name**\. Enter the Fully Qualified Domain Name \(FQDN\) of the [ forest root domain ](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/selecting-the-forest-root-domain) used for the Active Directory\. When you choose to create a new Active Directory, Launch Wizard creates a domain admin user on your Active Directory\.

**Creating a new AWS Managed Active Directory through Launch Wizard**
   + **Domain user name and password**\. The domain user name is preset to “admin\.” Enter a password for your directory\. Launch Wizard stores the password in AWS Secrets Manager as a secure string parameter\. It does not store the password on the server side\. To create a functional SQL Server Always On deployment, it reads from AWS Secrets Manager\.
   + **Domain DNS name**\. Enter a Fully Qualified Domain Name \(FQDN\) of the forest root domain used for the Active Directory\. When you choose to create a new Active Directory, Launch Wizard creates a domain admin user on your Active Directory\.

**Connecting to a self\-managed Active Directory through Launch Wizard**  
Launch Wizard allows you to connect to a self\-managed Active Directory environment during deployment\. For more information, see [Self\-managed Active Directory](launch-wizard-setting-up.md#launch-wizard-ad-onprem)\.

------
#### [ SQL Server ]

   When you use an existing Active Directory, you have the option of using an existing SQL Server service account or creating a new account\. If you create a new Active Directory account, you must create a new SQL Server account\. 
   + **User name and password**\. If you are using an existing SQL Server service account, provide your user name and password\. This SQL Server service account should be part of the Managed Active Directory in which you are deploying\. If you are creating a new SQL Server service account through Launch Wizard, enter a user name for the SQL Server service account\. Create a complex Password that is at least 8 characters long, and then reenter the password to verify it\. See [Password Policy](https://docs.microsoft.com/en-us/sql/relational-databases/security/password-policy?view=sql-server-2017) for more information\.
   + **SQL Server install type**\. Select the version of SQL Server Enterprise that you want to deploy\. You can select an AMI from either the License\-included AMI or Custom AMI dropdown lists\.
   + **License\-included AMI**\. Choose an AMI for your SQL Server deployment which determines the version and edition of Windows Server and SQL Server that will be deployed\.
   + **tempdb configuration \(optional\)**\. To improve performance, you can opt for the SQL Server tempdb system database to reside on a local NVMe SSD ephemeral storage device, also called the \(instance store volume\)\. NVMe SSD instance store volumes are available only on instance types that provide these local storage devices\. Additionally, only data that changes frequently should ever reside on these volumes\. They are not intended to store data long\-term\. For more information, see [Amazon EC2 instance store](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html)\.
   + **Additional SQL Server settings \(optional\)**\. You can optionally specify the following:
     + **Nodes**\. Enter a **Primary SQL node name** and a **Secondary SQL node name \(HA deployments only\)**\. 
     + **Additional secondary SQL node \(HA deployments only, maximum of 5\)**\. Enter a secondary **Node name**, and select the **Access type**, the **Private subnet**, and the **Dedicated host**, if applicable, for the additional secondary SQL node from the dropdown lists\. You can add more secondary nodes by selecting **Add additional secondary node**\. 
     + **Witness node \(optional, HA deployments only\)**\. For improved fault tolerance, select the check box to add a file share quorum witness node\.
     + **Additional naming**\. Enter a **Database name**\. For HA deployments, enter an **Availability group name**, a **Listener name**, and a **Windows cluster virtual network name**\. 

------

1. When you are satisfied with your configuration selections, select **Next**\. If you don't want to complete the configuration, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. To go to the previous screen, select **Previous**\.

1. After configuring your application, you are prompted to define the infrastructure requirements for the new deployment on the **Define infrastructure requirements** page\. The following tabs provide information about the input fields\.

------
#### [ Define infrastructure requirements ]

   You can choose to select your instances and volume types, or to use AWS recommended resources\. If you choose to use AWS recommended resources, you have the option of defining your high availability cluster needs\. If no selections are made, default values are assigned\.
   + **Number of instance cores**\. Choose the number of CPU cores for your infrastructure\. The default value assigned is 4\. 
   + **Network performance**\. Choose your preferred network performance in Gbps\.
   + **Memory \(GB\)**\. Choose the amount of RAM that you want to attach to your EC2 instances\. The default value assigned is 4 GB\.
   + **Type of storage drive**\. Select the storage drive type for the SQL data and tempdb volumes\. If you chose to place your tempdb on local storage, only the SQL data will be on the storage drive you select\. The default value assigned is SSD\. 
   + **SQL Server throughput**\. Select the sustained SQL Server throughput that you need\. 
   + **Recommended resources**\. Launch Wizard displays the system\-recommended resources based on your infrastructure selections\. If you want to change the recommended resources, select different infrastructure requirements\. 

**Infrastructure requirements based on instance type**

   You can choose to select your instance and volume type, or to use AWS recommended resources\. If no selections are made, default values are assigned\.
   + **Instance type**\. Select your preferred instance type from the dropdown list\. 
   + **Volume type**\. Choose your preferred EBS volume type\. For more information about volume types, see [Amazon EBS volume types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html)\.

**Drive letters and volume size**
   + **Drive letter**\. Select the storage drive letter for **Root drive**, **Logs**, **Data**, and **Backup** volumes\.
**Important**  
For custom AMIs, Launch Wizard assumes the root volume drive is `C:`\.
   + **Volume size**\. Select the size of the SQL Server data volume in Gb for **Root drive**, **Logs**, **Data**, and **Backup** volumes\. SQL Server logs and data will be staged on the same data volume for this deployment\. Make sure that you select an adequate size for the data volume\.

------
#### [ Tags\-Optional ]

   You can provide optional custom tags for the resources Launch Wizard creates on your behalf\. For example, you can set different tags for EC2 instances, EBS volumes, VPC, and subnets\. If you select **All**, you can assign a common set of tags to your resources\. Launch Wizard assigns tags with a fixed key `LaunchWizardResourceGroupID` and value that corresponds to the ID of the AWS resource group created for a deployment\. Launch Wizard does not support custom tagging for root volumes\. 

------
#### [ Estimated on\-demand cost to deploy additional resources ]

   AWS Launch Wizard provides an estimate for application charges incurred to deploy the selected resources\. The estimate updates each time you change a resource type in the Wizard\. The provided estimates are only for general comparisons\. They are based upon On\-Demand costs and your actual costs may be lower\. 

------

1. When you are satisfied with your infrastructure selections, select **Next**\. If you don't want to complete the configuration, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. To go to the previous screen, select **Previous**\.

1. On the **Review and deploy** page, review your configuration details\. If you want to make changes, select **Previous**\. To stop, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. When you choose **Deploy**, you agree to the terms of the **Acknowledgment**\.

1. Launch Wizard validates the inputs and notifies you of any issues you must address\. 

1. When validation is complete, Launch Wizard deploys your AWS resources and configures your SQL Server Always On application\. Launch Wizard provides you with status updates about the progress of the deployment on the **Deployments** page\. From the **Deployments** page, you can view the list of current and previous deployments\.

1. When your deployment is ready, a notification informs you that your SQL Server application is successfully deployed\. If you have set up an SNS notification, you are also alerted through SNS\. You can manage and access all of the resources related to your SQL Server Always On application by selecting the deployment, and then selecting **Manage** from the **Actions** dropdown list\.

1. When the SQL Server Always On application is deployed, you can access your Amazon EC2 instances through the EC2 console\. You can also use [AWS SSM](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html) to manage your SQL Server Always On application for future updates and patches through built\-in integration via resource groups\.

### Deploy SQL Failover Clustering application<a name="deploy-console-launch-wizard-failover-clustering"></a>

The following steps guide you through a SQL Failover Clustering application deployment with AWS Launch Wizard after you have launched it from the console\.

1. When you select **Choose application** from the AWS Launch Wizard landing page, you are directed to the **Choose application** wizard, where you are prompted to select the type of application that you want to deploy\. Select **Microsoft SQL Server**, then **Create deployment**\.

1. Under ** Review Permissions**, Launch Wizard displays the AWS Identity and Access Management \(IAM\) role required for Launch Wizard to access other AWS services on your behalf\. For more information about setting up IAM for Launch Wizard, see [AWS Identity and Access Management \(IAM\)](launch-wizard-setting-up.md#launch-wizard-iam)\. Choose **Next** \.

1. On the **Configure application settings** page, select the **Operating System** on which you want to install SQL Server — in this case, **Windows**\.

1. **Deployment model**\. Choose **High availability deployment**, and then choose **Always On Failover Cluster Instances** to deploy a SQL Server Failover Clustering \(FCI\) application across multiple Availability Zones\.

1. You are prompted to enter the specifications for the new deployment The following tabs provide information about the specification fields\.

------
#### [ General ]
   + **Deployment name**\. Enter a unique application name for your deployment\.
   + **Simple Notification Service \(SNS\) topic ARN — optional**\. Specify an SNS topic where AWS Launch Wizard can send notifications and alerts\. For more information, see the [https://docs.aws.amazon.com/sns/latest/dg/welcome.html](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\.
   + **CloudWatch application monitoring \(optional for HA deployments\)**\. Select the check box to set up monitors and automated insights for your deployment using CloudWatch Application Insights\. For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch-application-insights.html)\.
   + **Enable rollback on failed deployment**\. By default, if a deployment fails, your provisioned resources will not be rolled back/deleted\. This default configuration helps you to troubleshoot errors at the resource level as you debug deployment issues\. If you want your provisioned resources to be immediately deleted if a deployment fails, select the check box\.

------
#### [ Connectivity ]

   Enter the specifications for how you want to connect to your instance and configure your Virtual Private Cloud \(VPC\)\. 

   **Key pair name**
   + Select an existing key pair from the dropdown list or create a new one\. If you select **Create new key pair name**, you are directed to the Amazon EC2 console\. From there, under **Network and Security**, choose **Key Pairs**\. Choose **Create a new key pair**, enter a name for the key pair, and then choose **Download Key Pair**\.
**Important**  
This is the only opportunity for you to save the private key file\. Download it and save it in a safe place\. You must provide the name of your key pair when you launch an instance and provide the corresponding private key each time that you connect to the instance\. 

     Return to the Launch Wizard console and choose the refresh button next to the **Key Pairs** dropdown list\. The newly created key pair appears in the dropdown list\. For more information about key pairs, see [Amazon EC2 Key Pairs and Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\.

   **Tenancy model \(HA deployments only\)**

   Select your preferred tenancy\. Each instance that you launch into a VPC has a tenancy attribute\. The **Shared** tenancy option means that the instance runs on shared hardware\. The **Dedicated Host \(HA deployments\)** tenancy option means that the instance runs on a Dedicated Host, which is an isolated server with configurations that you can control\. For FCI deployments, select **Shared** tenancy\.

   **Virtual Private Cloud \(VPC\)**\. Choose whether you want to use an existing VPC or create a new VPC\.
   + **Select Virtual Private Cloud \(VPC\)** option\. Choose the VPC that you want to use from the dropdown list\. If you choose to enable Remote Desktop Gateway access, then your VPC must include at least one public subnet and two private subnets for HA deployments \. Your VPC must be associated with a [DHCP Options Set](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html) to enable DNS translations to work\. The private subnets must have outbound connectivity to the internet and other AWS services \(S3, CFN, SSM, Logs\)\. We recommend that you enable this connectivity with a NAT Gateway\. For more information about NAT Gateways, see [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) in the Amazon VPC User Guide\.
     + **Public Subnet**\. If you choose to enable Remote Desktop Gateway access, then your VPC must include at least one public subnet and two private subnets for HA deployments\. Choose a public subnet for your VPC from the dropdown list\. To continue, you must select the check box that indicates that the public subnet has been set up and the private subnets have outbound connectivity enabled\. 

**To add a new public subnet**

       If a subnet's traffic is routed to an internet gateway, the subnet is known as a public subnet\. If, however, a subnet doesn't have a route to the internet gateway, the subnet is known as a private subnet\. To use an existing VPC that does not have a public subnet, you can add a new public subnet using the following steps\.
       + Follow the steps in [Creating a Subnet in the Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Create_Subnet) using the existing VPC you intend to use AWS Launch Wizard\.
       + To add an internet gateway to your VPC, follow the steps in [Attaching an Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Attach_Gateway) in the Amazon VPC User Guide\.
       + To configure your subnets to route internet traffic through the internet gateway, follow the steps in [Creating a Custom Route Table](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Routing) in the Amazon VPC User Guide\. Use IPv4 format \(0\.0\.0\.0/0\) for Destination\.
       + The public subnet should have the “auto\-assign public IPv4 address” setting enabled\. To enable this setting, follow the steps in [Modifying the Public IPv4 Addressing Attribute for Your Subnet](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-ip-addressing.html#subnet-public-ip) in the Amazon VPC User Guide\.
     + **Availability Zone \(AZ\) configuration**\. You must choose at least two Availability Zones for High Availability \(HA\) deployments, with one private subnet for each zone that you select\. For HA deployments, select the **Availability Zones** within which you want to deploy your **primary** and **secondary** SQL nodes\. Depending on the number of secondary nodes that you plan to use to set up a SQL Server Always On deployment, you may have to specify a ** private subnet** for each of them\. Cross\-Region replication is not supported\. 

**To create a private subnet**

       If a subnet doesn't have a route to an internet gateway, the subnet is known as a private subnet\. To create a private subnet, you can use the following steps\. We recommend that you enable the outbound connectivity for each of your selected private subnets using a NAT Gateway\. To enable outbound connectivity from private subnets with public subnet, see the steps in [Creating a NAT Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-creating) to create a NAT Gateway in your chosen public subnet\. Then, follow the steps in [Updating Your Route Table ](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-create-route)for each of your chosen private subnets\.
       + Follow the steps in [Creating a Subnet](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#AddaSubnet) in the Amazon VPC User Guide using the existing VPC you will use in AWS Launch Wizard\. 
       + When you create a VPC, it includes a main route table by default\. On the **Route Tables** page in the Amazon VPC console, you can view the main route table for a VPC by looking for Yes in the Main column\. The main route table controls the routing for all subnets that are not explicitly associated with any other route table\. If the main route table for your VPC has an outbound route to an internet gateway, then any subnet created using the previous step, by default, becomes a public subnet\. To ensure the subnets are private, you may need to create separate route table\(s\) for your private subnets\. These route tables must not contain any routes to an internet gateway\. Alternatively, you can create a custom route table for your public subnet and remove the internet gateway entry from the main route table\.
     + **Remote Desktop Gateway preferences**\. When you select **Set up Remote Desktop Gateway**, enter the public subnet into which to deploy the RDGW instance\.
     + **Remote Desktop Gateway access — Optional**\. Select **Custom IP** from the dropdown list\. Enter the CIDR block\. If you do not specify any value for the Custom IP parameter, Launch Wizard does not set the inbound RDP access \(Port 3389\) from any IP\. You can choose to do this later by modifying the security group settings via the Amazon EC2 console\. See [Adding a Rule for Inbound RDP Traffic to a Windows Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html#add-rule-authorize-access) for instructions on adding a rule that allows inbound RDP traffic to your RDGW instance\. 
   + **Create new Virtual Private Cloud \(VPC\)** option\. Launch Wizard creates your VPC\. You can optionally enter a **VPC name tag**\. 
     + **Remote Desktop Gateway preferences**\. When you select **Set up Remote Desktop Gateway**, only the Remote Desktop Gateway access information will be taken from the VPC\.
     + **Remote Desktop Gateway access — Optional**\. Select **Custom IP** from the dropdown list\. Enter the CIDR block\. If you do not specify any value for the Custom IP parameter, Launch Wizard does not set the inbound RDP access \(Port 3389\) from any IP\. You can choose to do this later by modifying the security group settings via the Amazon EC2 Console\. See [Adding a Rule for Inbound RDP Traffic to a Windows Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html#add-rule-authorize-access) for instructions on adding a rule that allows inbound RDP traffic to your RDGW instance\. 

------
#### [ Active Directory ]

   You can connect to an existing Active Directory or create a new one\. If you selected the **Create new Virtual Private Cloud \(VPC\)** option for high availability deployments, you must select **Create a new Active Directory**\.

**Connecting to existing AWS Managed Active Directory or self\-managed Active Directory**

   From the dropdown list, select whether you want to use **AWS Managed Active Directory**, or **Self\-managed Active Directory**\. If you select **Self\-managed Active Directory**, select the check box to verify that you have ensured a connection between the Active Directory and the VPC\.

   Follow the steps for granting permissions in the Active Directory Default Organizational Unit \(OU\)\. 
   + **Domain user name and password**\. Enter the user name and password for your directory\. For required permissions for the domain user, see [Active Directory \(Windows deployment\)Active Directory \(Windows\)](launch-wizard-setting-up.md#launch-wizard-ad)\. Launch Wizard stores the password in AWS Secrets Manager as a secure string parameter\. It does not store the password on the service side\. To create a functional SQL Server FCI deployment, Launch Wizard reads from AWS Secrets Manager\.
   + **DNS address**\. Enter the IP address of the DNS servers to which you are connecting\. These servers must be reachable from within the VPC that you selected\. 
   + **Optional DNS address**\. If you would like to use a backup DNS server, enter the IP address of the DNS server that you want to use as backup\. These servers must be reachable from within the VPC that you selected\. 
   + **Domain DNS name**\. Enter the Fully Qualified Domain Name \(FQDN\) of the [ forest root domain ](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/selecting-the-forest-root-domain) used for the Active Directory\. When you choose to create a new Active Directory, Launch Wizard creates a domain admin user on your Active Directory\.
   + **Domain User security group — optional**\. To specify an existing security group, select one from the dropdown list\. The prerequisites for adding security groups can be viewed by selecting **Info**\.

**Creating a new AWS Managed Active Directory through Launch Wizard**
   + **Domain user name and password**\. The domain user name is preset to “admin\.” Enter a password for your directory\. Launch Wizard stores the password in AWS Secrets Manager as a secure string parameter\. It does not store the password on the server side\. To create a functional SQL Server FCI deployment, Launch Wizard reads from AWS Secrets Manager\.
   + **Domain DNS name**\. Enter a Fully Qualified Domain Name \(FQDN\) of the forest root domain used for the Active Directory\. When you choose to create a new Active Directory, Launch Wizard creates a domain admin user on your Active Directory\.

**Connecting to a self\-managed Active Directory through Launch Wizard**  
Launch Wizard allows you to connect to a self\-managed Active Directory environment during deployment\. For more information, see [Self\-managed Active Directory](launch-wizard-setting-up.md#launch-wizard-ad-onprem)\.

------
#### [ SQL Server ]

   When you use an existing Active Directory, you have the option of using an existing SQL Server service account or creating a new account\. If you create a new Active Directory account, you must create a new SQL Server account\. 
   + **User name and password**\. If you are using an existing SQL Server service account, provide your user name and password\. This SQL Server service account should be part of the Managed Active Directory in which you are deploying\. If you are creating a new SQL Server service account through Launch Wizard, enter a user name for the SQL Server service account\. Create a complex Password that is at least 8 characters long, and then reenter the password to verify it\. See [Password Policy](https://docs.microsoft.com/en-us/sql/relational-databases/security/password-policy?view=sql-server-2017) for more information\.
   + **SQL Server install type**\. Select the version of SQL Server Enterprise that you want to deploy\. You can select an AMI from either the License\-included AMI or Custom AMI dropdown lists\.
   + **License\-included AMI**\. Choose an AMI for your SQL Server deployment which determines the version and edition of Windows Server and SQL Server that will be deployed\.
   + **Additional SQL Server settings \(optional\)**\. You can optionally specify the following:
     + **Nodes**\. Enter a **Primary SQL node name** and a **Secondary SQL node name**\. 
     + **Additional naming**\. Enter a **SQL Server virtual network name** and a **Windows cluster virtual network name**\. 

------

1. When you are satisfied with your configuration selections, select **Next**\. If you don't want to complete the configuration, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. To go to the previous screen, select **Previous**\.

1. After configuring your application, you are prompted to define the infrastructure requirements for the new deployment on the **Define infrastructure requirements** page\. The following tabs provide information about the input fields\.

------
#### [ Define infrastructure requirements ]

   You can choose to select your instances and volume types, or to use AWS recommended resources\. If you choose to use AWS recommended resources, you have the option of defining your high availability cluster needs\. If no selections are made, default values are assigned\.

   **Instances**
   + **Cores**\. Choose the number of CPU cores for your infrastructure\. The default value assigned is 4\. 
   + **Network performance**\. Choose your preferred network performance in Gbps\.
   + **Memory \(GB\)**\. Choose the amount of RAM that you want to attach to your EC2 instances\. The default value assigned is 4 GB\.

   **Storage and performance**
   + **Type of storage drive**\. The default value assigned is SSD for FCI application deployments\.
   + **Average and peak IOPS**\. Select the average and peak IOPS required for your FSx share\.
   + **Allocated storage space**\. Select the amount of storage required for your FSx drive\.
   + **Recommended resources**\. Launch Wizard displays the system\-recommended resources based on your infrastructure selections\. If you want to change the recommended resources, select different infrastructure requirements\. 

**Infrastructure requirements based on instance type**

   You can choose to select your instance and storage capacity, or to use AWS recommended resources\. If no selections are made, default values are assigned\.
   + **Instance type**\. Select your preferred instance type from the dropdown list\. 
   + **Storage capacity**\. Choose your preferred EBS volume type\. For more information about volume types, see [Amazon EBS volume types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html)\.
   + **Throughput capacity**\. Select the required sustained SQL Server throughput\.

------
#### [ Tags\-Optional ]

   You can provide optional custom tags for the resources Launch Wizard creates on your behalf\. For example, you can set different tags for EC2 instances, EBS volumes, VPC, and subnets\. If you select **All**, you can assign a common set of tags to your resources\. Launch Wizard assigns tags with a fixed key `LaunchWizardResourceGroupID` and value that corresponds to the ID of the AWS resource group created for a deployment\. Launch Wizard does not support custom tagging for root volumes\. 

------
#### [ Estimated on\-demand cost to deploy additional resources ]

   AWS Launch Wizard provides an estimate for application charges incurred to deploy the selected resources\. The estimate updates each time you change a resource type in the Wizard\. The provided estimates are only for general comparisons\. They are based upon On\-Demand costs and your actual costs may be lower\. 

------

1. When you are satisfied with your infrastructure selections, select **Next**\. If you don't want to complete the configuration, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. To go to the previous screen, select **Previous**\.

1. On the **Review and deploy** page, review your configuration details\. If you want to make changes, select **Previous**\. To stop, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. When you choose **Deploy**, you agree to the terms of the **Acknowledgment**\.

1. Launch Wizard validates the inputs and notifies you of any issues you must address\. 

1. When validation is complete, Launch Wizard deploys your AWS resources and configures your SQL Server FCI application\. Launch Wizard provides you with status updates about the progress of the deployment on the **Deployments** page\. From the **Deployments** page, you can view the list of current and previous deployments\.

1. When your deployment is ready, a notification informs you that your SQL Server application is successfully deployed\. If you have set up an SNS notification, you are also alerted through SNS\. You can manage and access all of the resources related to your SQL Server FCI application by selecting the deployment, and then selecting **Manage** from the **Actions** dropdown list\.

1. When the SQL Server FCI application is deployed, you can access your Amazon EC2 instances through the EC2 console\. You can also use [AWS SSM](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html) to manage your SQL Server FCI application for future updates and patches through built\-in integration via resource groups\.