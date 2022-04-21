# Deploy IIS into an existing Amazon Virtual Private Cloud<a name="launch-wizard-iis-deployment-steps-existing-vpc"></a>

The following steps guide you through an IIS deployment with AWS Launch Wizard after you have launched it from the console for an existing VPC\.

**Important**  
When you deploy IIS to an existing Amazon VPC, you must have an existing Active Directory domain\. Also, the Domain Name System \(DNS\) must allow for the discovery of the Active Directory domain, such as with [VPC DHCP options sets](https://docs.aws.amazon.com/vpc/latest/userguide/WhatAreDHCPOptionSets.html)\.

1. When you select **Choose application** from the AWS Launch Wizard landing page, you are directed to the Choose application wizard where you are prompted to select the type of application that you want to deploy\.

1. Select **Internet Information Services**, select the deployment type, then select **Create deployment\.**

1. You are prompted to enter the specifications for the new deployment\. The following tabs provide information about the specification fields of the deployment model\.

------
#### [ General settings ]
   + **Deployment name**\. Enter a unique application name for your deployment\.
   + **Amazon Simple Notification Service \(Amazon SNS\) topic ARN — optional**\. Specify an Amazon SNS topic where AWS Launch Wizard can send notifications and alerts\. For more information, see the [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\.
   + **Deactivate rollback on failed deployment**\. By default, if a deployment fails, your provisioned resources will be deleted\. You can enable this setting during deployment to prevent this behavior\.
   + **Tags \- optional**\. Enter a key and value to assign metadata to your deployment\. For help with tagging, see [Tagging Your Amazon EC2 Resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html)\.

------
#### [ Network configuration ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-iis-deployment-steps-existing-vpc.html)

------
#### [ Active Directory configuration ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-iis-deployment-steps-existing-vpc.html)

**Note**  
The domain administrator user name is separate from the default administrator account\.

------
#### [ IIS webpage ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-iis-deployment-steps-existing-vpc.html)

------
#### [ Auto Scaling group/ELB configuration ]    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-iis-deployment-steps-existing-vpc.html)

------

1. When you are satisfied with your infrastructure selections, select **Next**\. If you don't want to complete the configuration, select **Cancel**\. If you cancel, all of the selections on the specification page are lost and you are returned to the landing page\. To go to the previous screen, select **Previous**\.

1. On the **Review and deploy** page, review your configuration details\. If you want to make changes, select **Previous**\. To stop, select **Cancel**\. When you select **Cancel**, all of the selections on the specification page are lost and you are returned to the landing page\. When you choose **Deploy**, you agree to the terms of the **Acknowledgment**\. Launch Wizard validates the inputs and notifies you of any issues you must address\. 

1. When validation is complete, Launch Wizard deploys your AWS resources and configures your **IIS** application\. Launch Wizard provides you with status updates about the progress of the deployment on the **Deployments** page\. From the **Deployments** page, you can view the list of current and previous deployments\. 

1. When your deployment is ready, a notification informs you that the **IIS** application is successfully deployed\. If you have set up an Amazon SNS notification, you are also alerted through Amazon SNS\. To manage and access all of the resources related to your application, select the deployment, and from the **Actions** dropdown list, select **Manage**\. 

1. When the application is deployed, you can access your instances through the Amazon EC2 console\.