# Deploy Exchange Server into a new Amazon Virtual Private Cloud<a name="launch-wizard-exchange-deployment-new-vpc"></a>

The following steps guide you through an Exchange Server deployment with AWS Launch Wizard after you have launched it from the console\.

1. On the AWS Launch Wizard Console's landing page, use the **Choose application** button\. This opens the Choose application wizard where you are prompted to select the type of application that you want to deploy\.

1. Select **Exchange**, select **Deploy Exchange into a new VPC**, then select **Create deployment\.**

1. You are prompted to enter the specifications for the new deployment\. The following tabs provide information about the specification fields of the deployment model\.

------
#### [ General ]
   + **Deployment name**\. Enter a unique application name for your deployment\.
   + **Amazon Simple Notification Service \(Amazon SNS\) topic ARN — optional**\. Specify an Amazon SNS topic where AWS Launch Wizard can send notifications and alerts\. For more information, see the [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\.
   + **Deactivate rollback on failed deployment**\. By default, if a deployment fails, your provisioned resources will be deleted\. You can enable this setting during deployment to prevent this behavior\.
   + **Tags \- optional**\. Enter a key and value to assign metadata to your deployment\. For help with tagging, see [Tagging Your Amazon EC2 Resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html)\.

------
#### [ Basic configuration ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-exchange-deployment-new-vpc.html)

------
#### [ Network configuration ]
   + **Key pair name**\. Select an existing key pair from the dropdown list or create a new one\. If you select **Create new key pair name**, you are directed to the Amazon EC2 console\. From there, under **Network and Security**, choose **Key Pairs**\. Choose **Create a new key pair**, enter a name for the key pair, and then choose **Download Key Pair**\.
**Important**  
This is the only opportunity for you to save the private key file\. Download it and save it in a safe place\. You must provide the name of your key pair when you launch an instance and provide the corresponding private key each time that you connect to the instance\. Return to the Launch Wizard console and choose the refresh button next to the **Key Pairs** dropdown list\. The newly created key pair appears in the dropdown list\. For more information about key pairs and Linux instances, see [Amazon EC2 Key Pairs and Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\. For more information about key pairs and Windows instances, see [Amazon EC2 Key Pairs and Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-key-pairs.html)
   + **Allowed external access CIDR:** Allowed CIDR block for external access to the deployed instances\.
   + **VPC settings:** Launch Wizard creates your VPC in this case\. Input fields that define the VPC configuration are shown in the following list\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-exchange-deployment-new-vpc.html)

------
#### [ Microsoft Active Directory Configuration ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-exchange-deployment-new-vpc.html)

------
#### [ Remote Desktop Gateway Configuration ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-exchange-deployment-new-vpc.html)

------
#### [ Exchange Server Configuration ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-exchange-deployment-new-vpc.html)

------
#### [ Load Balancer Configuration ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-exchange-deployment-new-vpc.html)

------
#### [ Failover Cluster Configuration ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-exchange-deployment-new-vpc.html)

------

1. When you are satisfied with your infrastructure selections, choose **Next**\. If you don't want to complete the configuration, choose **Cancel**\. When you choose **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. To return to the previous screen, choose **Previous**\.

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

1. When validation is complete, Launch Wizard deploys your AWS resources and configures your **Exchange** application\. Launch Wizard provides you with status updates about the progress of the deployment on the **Deployments** page\. From the **Deployments** page, you can view the list of current and previous deployments\. 

1. When your deployment is ready, a notification informs you that your **Exchange** application is successfully deployed\. If you have set up an Amazon SNS notification, you are also alerted through Amazon SNS\. You can manage and access all of the resources related to your application by selecting the deployment, and then selecting **Manage** from the **Actions** dropdown list\. 

1. When the application is deployed, you can access your EC2 instances through the Amazon EC2 console\.