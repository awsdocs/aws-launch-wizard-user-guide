# Deploy standalone Remote Desktop Gateway into a new Amazon Virtual Private Cloud<a name="launch-wizard-remote-desktop-gateway-deployment-steps-new-vpc"></a>

The following steps guide you through a Remote Desktop Gateway deployment with AWS Launch Wizard after you have launched it from the console\.

1. When you select **Choose application** from the AWS Launch Wizard landing page, you are directed to the Choose application wizard where you are prompted to select the type of application that you want to deploy\.

1. Select **Microsoft Remote Desktop Gateway**, select **Deploy into a new VPC**, then select **Create deployment\.**

1. You are prompted to enter the specifications for the new deployment\. The following tabs provide information about the specification fields of the deployment model\.

------
#### [ General ]
   + **Deployment name**\. Enter a unique application name for your deployment\.
   + **Amazon Simple Notification Service \(SNS\) topic ARN — optional**\. Specify an Amazon SNS topic where AWS Launch Wizard can send notifications and alerts\. For more information, see the [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\.
   + **Deactivate rollback on failed deployment**\. By default, if a deployment fails, your provisioned resources will be deleted\. You can enable this setting during deployment to prevent this behavior\.
   + **Tags \- optional**\. Enter a key and value to assign metadata to your deployment\. For help with tagging, see [Tagging Your EC2 Resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html)\.

------
#### [ Network configuration ]
   + **Key pair name**\. Select an existing key pair from the dropdown list or create a new one\. If you select **Create new key pair name**, you are directed to the Amazon EC2 console\. From there, under **Network and Security**, choose **Key Pairs**\. Choose **Create a new key pair**, enter a name for the key pair, and then choose **Download Key Pair**\.
**Important**  
This is the only opportunity for you to save the private key file\. Download it and save it in a safe place\. You must provide the name of your key pair when you launch an instance and provide the corresponding private key each time that you connect to the instance\. Return to the Launch Wizard console and choose the refresh button next to the **Key Pairs** dropdown list\. The newly created key pair appears in the dropdown list\. For more information about key pairs, see [Amazon EC2 Key Pairs and Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\.
   + **Availability Zone \(AZ\) configuration:** You must choose at least two Availability Zones\. Deployment will create a highly available architecture that spans these Availability Zones\.
   + **VPC Settings: **Launch Wizard creates your VPC in this case\. The following shows Input fields that define VPC configuration\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-remote-desktop-gateway-deployment-steps-new-vpc.html)

------
#### [ Microsoft Remote Desktop Gateway configuration ]

       
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-remote-desktop-gateway-deployment-steps-new-vpc.html)

------

1. When you are satisfied with your infrastructure selections, select **Next**\. If you don't want to complete the configuration, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. To go to the previous screen, select **Previous**\.

1. After configuring your application, you are prompted to define the infrastructure requirements for the new deployment on the **Define infrastructure requirements** page\. The following tabs provide information about the input fields\.

------
#### [ Compute ]
   + **Infrastructure requirements based on instance type**\. You can choose to select your instances or use AWS recommended resources\. If you choose to use AWS recommended resources, you have the option of defining your performance needs\. If no selections are made, default values are assigned\.
   + **Number of instance cores**\. Choose the number of CPU cores for your infrastructure\. The default value assigned is 4\.
   + **Network performance**\. Choose your preferred network performance in Gbps\.
   + **Memory \(GB\)**\. Choose the amount of RAM that you want to attach to your EC2 instances\. The default value assigned is 4 GB\.
   + **Recommended resources**\. Launch Wizard displays the system\-recommended resources based on your infrastructure selections\. If you want to change the recommended resources, select different infrastructure requirements\.
   + **Infrastructure requirements based on instance type**\. You can choose to select your instance or use AWS recommended resources\. If no selections are made, default values are assigned\.
   + **Instance type**\. Select your preferred instance type from the dropdown list\.

------

1. When you are satisfied with your infrastructure selections, select **Next**\. If you don't want to complete the configuration, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. To go to the previous screen, select **Previous**\.

1. On the **Review and deploy** page, review your configuration details\. If you want to make changes, select **Previous**\. To stop, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. When you choose **Deploy**, you agree to the terms of the **Acknowledgment**\. Launch Wizard validates the inputs and notifies you of any issues you must address\. 

1. When validation is complete, Launch Wizard deploys your AWS resources and configures your **Microsoft Remote Desktop Gateway** application\. Launch Wizard provides you with status updates about the progress of the deployment on the **Deployments** page\. From the **Deployments** page, you can view the list of current and previous deployments 

1. When your deployment is ready, a notification informs you that your **Remote Desktop Gateway** application is successfully deployed\. If you have set up an Amazon SNS notification, you are also alerted through Amazon SNS\. To manage and access all of the resources related to your application, select the deployment, and from the **Actions** dropdown list, select **Manage**\. 

1. When the application is deployed, you can access your EC2 instances through the Amazon EC2 console\.