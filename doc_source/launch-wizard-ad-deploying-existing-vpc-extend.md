# Extend on\-premises Active Directory to an existing Amazon Virtual Private Cloud<a name="launch-wizard-ad-deploying-existing-vpc-extend"></a>

The following steps guide you through an Active Directory deployment with AWS Launch Wizard after you have launched it from the console for an existing VPC\.

1. On the Launch Wizard console's landing page, use the **Choose application** button\. This opens the Choose application wizard where you are prompted to select the type of application that you want to deploy\.

1. Select **Active Directory**, select **Extend on\-premises AD into an existing VPC**, then select **Create deployment\.**

1. Review and acknowledge that the required IAM permissions are met before proceeding\. For more information, see [Identity and Access Management for AWS Launch Wizard](launch-wizard-security.md#identity-access-management)\.

1. When prompted, enter the specifications for the new deployment\. The following tabs provide information about the specification fields of the deployment model\.

------
#### [ General settings ]
   + **Deployment name**\. Enter a unique application name for your deployment\.
   + **Amazon Simple Notification Service \(Amazon SNS\) topic ARN — optional**\. Specify an Amazon SNS topic where Launch Wizard can send notifications and alerts\. For more information, see the [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\.
   + **Deactivate rollback on failed deployment**\. By default, if a deployment fails, your provisioned resources will be deleted\. You can enable this setting during deployment to prevent this behavior\.
   + **Tags \- optional**\. Enter a key and value to assign metadata to your deployment\. For help with tagging, see [Tagging Your Amazon EC2 Resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html)\.

------
#### [ Network configuration ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-ad-deploying-existing-vpc-extend.html)

------
#### [ Amazon EC2 configuration ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-ad-deploying-existing-vpc-extend.html)

------
#### [ Microsoft Active Directory Domain Services configuration ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-ad-deploying-existing-vpc-extend.html)

------

1. When you are satisfied with your application settings, choose **Next**\. If you don't want to complete the configuration, choose **Cancel**\. When you choose **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. To return to the previous screen, choose **Previous**\.

1. On the **Configure infrastructure settings** page, you are prompted to define the infrastructure settings for the new deployment\. The following tab provides information about the input fields\.

------
#### [ Storage and compute ]

   You can choose to select your instances, or to use AWS recommended resources\. If you choose to use AWS recommended resources, you have the option of defining your performance needs\. If you don't select either option, default values are assigned\. Launch Wizard will display the estimated charges incurred to deploy the application based on suggested infrastructure and also based on static values\.
   + **Based on infrastructure suggestion**\. Launch Wizard displays the suggested resources for the deployment\. You can specify your performance requirements of the resources to update the recommendation\.
     + **Number of instance cores**\. Choose the number of CPU cores for your infrastructure\. The default value assigned is 4\.
     + **Network performance**\. Choose your preferred network performance in Gbps\.
     + **Memory \(GB\)**\. Choose the amount of RAM that you want to attach to your EC2 instances\. The default value assigned is 4 GB\.
     + **Recommended resources**\. Launch Wizard displays the system\-recommended resources based on your infrastructure selections\. If you want to change the recommended resources, select different infrastructure settings\.
     +  **Estimated on\-demand cost to deploy additional resources**\. Launch Wizard displays the estimated charges incurred to deploy the resources\.
   + **Based on static values**\. You can specify specific instance types for the resources used in your deployment\. If you don't select either option, default values are assigned\.
     + **Instance type**\. You can choose your instance type from the dropdown list, or you can use AWS recommended resources\.
     +  **Estimated on\-demand cost to deploy additional resources**\. Launch Wizard displays the estimated charges incurred to deploy the resources\.

------

1. When you are satisfied with your infrastructure settings, select **Next**\. If you don't want to complete the configuration, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. To go to the previous screen, select **Previous**\.

1. On the **Review and deploy** page, review your configuration details\. If you want to make changes, select **Previous**\. To stop, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. When you choose **Deploy**, you agree to the terms of the **Acknowledgment**\. Launch Wizard validates the inputs and notifies you if you need to address any issues\. 

1. When validation is complete, Launch Wizard deploys your AWS resources and configures your application\. Launch Wizard provides you with status updates about the progress of the deployment on the **Deployments** page\. From the **Deployments** page, you can view the list of current and previous deployments\. 

1. When your deployment is ready, a notification informs you that your application is successfully deployed\. If you have set up an Amazon SNS notification, you are also alerted through Amazon SNS\. You can manage and access all of the resources related to your application by selecting the deployment, and then selecting **Manage** from the **Actions** dropdown list\. 

1. When the application is deployed, you can access your EC2 instances through the Amazon EC2 console\.