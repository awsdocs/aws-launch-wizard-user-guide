# Deploy IIS into a new Amazon Virtual Private Cloud<a name="launch-wizard-iis-deployment-steps-new-vpc"></a>

The following steps guide you through an IIS deployment with AWS Launch Wizard after you have launched it from the console for a new VPC\.

1. On the AWS Launch Wizard Console's landing page, use the **Choose application** button\. This opens the Choose application wizard where you are prompted to select the type of application that you want to deploy\.

1. Select **Internet Information Services**, select **Deploy into a new VPC**, then select **Create deployment\.**

1. You are prompted to enter the specifications for the new deployment\. The following tabs provide information about the specification fields of the deployment model\.

------
#### [ General settings ]
   + **Deployment name**\. Enter a unique application name for your deployment\.
   + **Amazon Simple Notification Service \(Amazon SNS\) topic ARN — optional**\. Specify an Amazon SNS topic where AWS Launch Wizard can send notifications and alerts\. For more information, see the [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\.
   + **Deactivate rollback on failed deployment**\. By default, if a deployment fails, your provisioned resources will be deleted\. You can enable this setting during deployment to prevent this behavior\.
   + **Tags \- optional**\. Enter a key and value to assign metadata to your deployment\. For help with tagging, see [Tagging Your Amazon EC2 Resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html)\.

------
#### [ Network configuration ]

   **Key pair name**\. Select an existing key pair from the dropdown list or create a new one\. If you select **Create new key pair name**, you are directed to the Amazon EC2 console\. From there, under **Network and Security**, choose **Key Pairs**\. Choose **Create a new key pair**, enter a name for the key pair, and then choose **Download Key Pair**\.

**Important**  
This is the only opportunity for you to save the private key file\. Download it and save it in a safe place\. You must provide the name of your key pair when you launch an instance and provide the corresponding private key each time that you connect to the instance\. Return to the Launch Wizard console and choose the refresh button next to the **Key Pairs** dropdown list\. The newly created key pair appears in the dropdown list\. For more information about key pairs, see [Amazon EC2 Key Pairs and Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-iis-deployment-steps-new-vpc.html)

------
#### [ Active Directory configuration ]

   **Active Directory scenario type**\. Select the type of deployment to use, either **AWS Directory Service for Microsoft Active Directory** or **Microsoft AD on Amazon EC2** to manage your own Amazon EC2 Active Directory instances\.

   These parameters are presented when you choose **AWS Directory Service for Microsoft Active Directory** for the **Active Directory scenario type**\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-iis-deployment-steps-new-vpc.html)

   These parameters are presented when you choose **Microsoft AD on EC2** for the **Active Directory scenario type**\.

**Note**  
The domain administrator user name is separate from the default administrator account\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-iis-deployment-steps-new-vpc.html)

------
#### [ RD Gateway configuration ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-iis-deployment-steps-new-vpc.html)

------
#### [ IIS webpage ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-iis-deployment-steps-new-vpc.html)

------
#### [ Auto Scaling group/ELB configuration ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-iis-deployment-steps-new-vpc.html)

------

1. When you are satisfied with your infrastructure selections, select **Next**\. If you don't want to complete the configuration, select **Cancel**\. If you cancel, all of the selections on the specification page are lost and you are returned to the landing page\. To go to the previous screen, select **Previous**\.

1. On the **Review and deploy** page, review your configuration details\. If you want to make changes, select **Previous**\. To stop, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. When you choose **Deploy**, you agree to the terms of the **Acknowledgment**\. Launch Wizard validates the inputs and notifies you of any issues you must address\. 

1. When validation is complete, Launch Wizard deploys your AWS resources and configures your **IIS** application\. Launch Wizard provides you with status updates about the progress of the deployment on the **Deployments** page\. From the **Deployments** page, you can view the list of current and previous deployments\. 

1. When your deployment is ready, a notification informs you that the **IIS** application is successfully deployed\. If you have set up an Amazon SNS notification, you are also alerted through Amazon SNS\. To manage and access all of the resources related to your application, select the deployment, and from the **Actions** dropdown list, select **Manage**\. 

1. When the application is deployed, you can access your instances through the Amazon EC2 console\.