# Manage application resources with AWS Launch Wizard for Active Directory<a name="launch-wizard-ad-managing"></a>

After you deploy your self\-managed domain controllers, you can manage them by following these steps\.

1. From the navigation pane, choose **Deployments**\.

1. From the **Deployments** page, select **Actions**\. You can select to do the following:

   1. **Manage resources on the EC2 console**\. You are taken to the Amazon EC2 console, where you can view and manage your domain controller resources\. For example, you can view and manage EC2, Amazon EBS, Active Directory, VPC, subnets, NAT Gateways, and Elastic IPs\.

   1. **View resource group with SSM**\. You are taken to the Systems Manager console to view your resource groups\.

   1. **View CloudWatch application logs**\. You are taken to CloudWatch Logs, where you can monitor, store, and access your SQL Server Always On application log files\. 

   1. **View your CloudFormation template**\. This is the CloudFormation template created by your most recent deployment, and it can be accessed through the CloudFormation console\. For help with finding and using your CloudFormation template, see [Viewing AWS CloudFormation Stack Data and Resources on the AWS Management Console](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-view-stack-data-resources.html)\.

1. To delete a deployment, select the application that you want to delete and select **Delete**\. You are prompted to confirm your action\.
**Important**  
You lose all specification settings for the domain controllers when you delete a deployment\. AWS Launch Wizard attempts to delete only the AWS resources that it created in your account as part of the deployment\. If you created resources outside of Launch Wizard, for example resources that reside in a VPC created by Launch Wizard, the deletion may fail\. Launch Wizard does not delete any Active Directory objects in your Active Directory, nor any of the records in your DNS server\. Launch Wizard has no control over your Active Directory domain user password over time, which is required to clean up Active Directory objects or DNS records\. We recommend that you remove these entries from your Active Directory after Launch Wizard deletes the deployment\. For key operations performed against your Active Directory resulting in new records or entries, see [Active Directory on EC2](launch-wizard-ad-setting-up.md#launch-wizard-ad-setup-managed)\.

1. To further investigate details regarding your domain controller resources, select the **Application name**\. You can then view the **Deployment events** and **Summary** details for your application by using the tabs at the top of the page\.