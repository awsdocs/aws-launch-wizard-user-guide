# Deploy Amazon Elastic Kubernetes Service into an existing Amazon Virtual Private Cloud<a name="launch-wizard-eks-deployment-existing-vpc"></a>

The following steps guide you through a Amazon EKS deployment with AWS Launch Wizard after you have launched it from the console\.

1. When you select **Choose application** from the AWS Launch Wizard landing page, you are directed to the Choose application wizard where you are prompted to select the type of application that you want to deploy\.

1. Select **Amazon EKS**, select **Deploy Amazon EKS into an existing VPC**, then select **Create deployment\.**

1. You are prompted to enter the specifications for the new deployment\. The following tabs provide information about the specification fields of the deployment model\.

------
#### [ General ]
   + **Deployment name**\. Enter a unique application name for your deployment\.
   + **Amazon Simple Notification Service \(SNS\) topic ARN — optional**\. Specify an Amazon SNS topic where AWS Launch Wizard can send notifications and alerts\. For more information, see the [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\.
   + **Enable rollback on failed deployment**\. By default, if a deployment fails, your provisioned resources will not be rolled back or deleted\. This default configuration helps you to troubleshoot errors at the resource level as you debug deployment issues\. If you want your provisioned resources to be immediately deleted if a deployment fails, select the check box\.
   + **Tags \(Optional\)**\. Enter a key and value to assign metadata to your deployment\. For help with tagging, see [Tagging Your Amazon EC2 Resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html)\.

------
#### [ Network configuration ]
   + **Key pair name**\. Select an existing key pair from the dropdown list or create a new one\. If you select **Create new key pair name**, you are directed to the Amazon EC2 console\. From there, under **Network and Security**, choose **Key Pairs**\. Choose **Create a new key pair**, enter a name for the key pair, and then choose **Download Key Pair**\.
**Important**  
This is the only opportunity for you to save the private key file\. Download it and save it in a safe place\. You must provide the name of your key pair when you launch an instance and provide the corresponding private key each time that you connect to the instance\. Return to the Launch Wizard console and choose the refresh button next to the **Key Pairs** dropdown list\. The newly created key pair appears in the dropdown list\. For more information about key pairs and Linux instances, see [Amazon EC2 Key Pairs and Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\. For more information about key pairs and Windows instances, see [Amazon EC2 Key Pairs and Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-key-pairs.html)
   + **Allowed external access CIDR:** Allowed CIDR block for external access to the deployed instances\.

   The following table shows all the input fields corresponding to the VPC, public subnets, and private subnets\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-eks-deployment-existing-vpc.html)

**VPC architecture requirements:**
   + **VPC ID:** Amazon EC2 is hosted in multiple locations world\-wide\. These locations are composed of AWS [Regions and Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)\. Amazon VPC enables you to launch AWS resources into a virtual network that you've defined\. Choose the VPC that you want to use from the dropdown list\. Your VPC must be associated at least two public subnets and two private subnets\.
   + **Availability Zone \(AZ\) configuration:** You must choose two or three Availability Zones in the Region\. Each of the Availability Zones will have a private subnet and a public subnet in the selected VPC\. A subnet is [a range of IP addresses within a VPC ](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) that is allocated in an Availability Zone for the Region\.
   + **Public Subnets**: You must choose at least two public subnets for your VPC\.

     If a subnet's traffic is routed to an internet gateway, it is a public subnet\. If a subnet doesn't have a route to the internet gateway, it is a private subnet\. To use an existing VPC that does not have a public subnet, add a new public subnet using the following steps\.
     + Follow the steps in [Creating a Subnet in the Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Create_Subnet) using the existing VPC that you intend to use in AWS Launch Wizard\.
     + Add an internet gateway to your VPC, by following the steps in [Attaching an Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Attach_Gateway) in the Amazon VPC User Guide\.
     + Configure your subnets to route internet traffic through the internet gateway, by following the steps in [Creating a Custom Route Table](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Routing) in the Amazon VPC User Guide\. Use IPv4 format \(0\.0\.0\.0/0\) for the destination\.
     + Enable the required public subnet setting of **auto\-assign public IPv4 address**\. To enable this setting, follow the steps in [Modifying the Public IPv4 Addressing Attribute for Your Subnet](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-ip-addressing.html#subnet-public-ip) in the Amazon VPC User Guide\.
**Important**  
You must [tag each public subnet](https://docs.aws.amazon.com/ARG/latest/userguide/tagging.html) being used with the key `kubernetes.io/role/elb` and the value `true`\.
   + **Private subnets:** You must choose at least two private subnets for your VPC\.

     If a subnet doesn't have a route to an internet gateway, the subnet is known as a private subnet\. To create a private subnet, you can use the following steps\. We recommend that you enable the outbound connectivity for each of your selected private subnets using a NAT Gateway\. To enable outbound connectivity from private subnets with public subnet, see the steps in [Creating a NAT Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-creating) to create a NAT Gateway in your chosen public subnet\. Then, follow the steps in [Updating Your Route Table ](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-create-route)for each of your chosen private subnets\.
     + Follow the steps in [Creating a Subnet](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#AddaSubnet) in the Amazon VPC User Guide using the existing VPC you will use in AWS Launch Wizard\. 
     + When you create a VPC, it includes a main route table by default\. On the **Route Tables** page in the Amazon VPC console, you can view the main route table for a VPC by looking for Yes in the Main column\. The main route table controls the routing for all subnets that are not explicitly associated with any other route table\. If the main route table for your VPC has an outbound route to an internet gateway, then any subnet created using the previous step, by default, becomes a public subnet\. To ensure that the subnets are private, you may need to create separate route tables for your private subnets\. These route tables must not contain any routes to an internet gateway\. Alternatively, you can create a custom route table for your public subnet and remove the internet gateway entry from the main route table\.
**Important**  
You must [tag each private subnet](https://docs.aws.amazon.com/ARG/latest/userguide/tagging.html) being used with the key `kubernetes.io/role/internal-elb` and the value `true`\.

------
#### [ EKS configuration ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-eks-deployment-existing-vpc.html)

------
#### [ EKS node group configuration ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-eks-deployment-existing-vpc.html)

------
#### [ Kubernetes add\-ins ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-eks-deployment-existing-vpc.html)

------

1. When you are satisfied with your infrastructure selections, select **Next**\. If you don't want to complete the configuration, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. To go to the previous screen, select **Previous**\.

1. After configuring your application, you are prompted to define the infrastructure requirements for the new deployment on the **Define infrastructure requirements** page\. The following tabs provide information about the input fields\.

------
#### [ Compute ]
   + **Infrastructure requirements based on infrastructure**\. You can choose to select your instances, or to use AWS recommended resources\. If you choose to use AWS recommended resources, you have the option of defining your performance needs\. If you don't select either option, default values are assigned\.
   + **Number of instance cores**\. Choose the number of CPU cores for your infrastructure\. The default value assigned is 4\.
   + **Network performance**\. Choose your preferred network performance in Gbps\.
   + **Memory \(GB\)**\. Choose the amount of RAM that you want to attach to your EC2 instances\. The default value assigned is 4 GB\.
   + **Recommended resources**\. Launch Wizard displays the system\-recommended resources based on your infrastructure selections\. If you want to change the recommended resources, select different infrastructure requirements\.
   + **Infrastructure requirements based on instance type**\. Choose to select your instance or to use AWS recommended resources\. If you don't select either option, default values are assigned\.
   + **Instance type**\. Select your preferred instance type from the dropdown list\.

------

1. When you are satisfied with your infrastructure selections, select **Next**\. If you don't want to complete the configuration, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. To go to the previous screen, select **Previous**\.

1. On the **Review and deploy** page, review your configuration details\. If you want to make changes, select **Previous**\. To stop, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. When you choose **Deploy**, you agree to the terms of the **Acknowledgment**\. Launch Wizard validates the inputs and notifies you if you need to address any issues\. 

1. When validation is complete, Launch Wizard deploys your AWS resources and configures your **Amazon EKS** application\. Launch Wizard provides you with status updates about the progress of the deployment on the **Deployments** page\. From the **Deployments** page, you can view the list of current and previous deployments 

1. When your deployment is ready, a notification informs you that your **Amazon EKS** application is successfully deployed\. If you have set up an Amazon SNS notification, you are also alerted through Amazon SNS\. You can manage and access all of the resources related to your application by selecting the deployment, and then selecting **Manage** from the **Actions** dropdown list\. 

1. When the application is deployed, you can access your Amazon EC2 instances through the Amazon EC2 Console\.