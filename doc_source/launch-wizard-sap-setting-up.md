# Set up for AWS Launch Wizard for SAP<a name="launch-wizard-sap-setting-up"></a>

This section describes the prerequisites that you must verify to deploy an SAP application with AWS Launch Wizard\. 

**Topics**
+ [General](#launch-wizard-sap-prerequisites)
+ [IAM](#launch-wizard-sap-iam)

## General prerequisites<a name="launch-wizard-sap-prerequisites"></a>

The following general prerequisites must be met to deploy an application with Launch Wizard\.
+ You must create an Amazon VPC that consists of private subnet\(s\) in a minimum of two Availability Zones\. The subnets must have outbound internet access\. For more information on how to create and set up a VPC, see [Getting Started with Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-getting-started.html) in the *Amazon VPC User Guide*\.
+ You must create an IAM user and attach the **AmazonLaunchWizard\_Fullaccess** policy\. See the [following sections](#launch-wizard-sap-iam) for the steps to attach the policy to the IAM user\.
+ To run custom pre\- and post\-configuration deployment scripts, you must add the permissions listed in [Add permissions to run custom pre\- and post\-deployment configuration scripts](#launch-wizard-sap-iam-scripts) to the `AmazonEC2RoleForLaunchWizard` role\. 
+ If you want to install an SAP HANA database, you must download the software from the SAP Software Download page and upload it to an Amazon S3 bucket\. For steps on how to download the software and upload it to an Amazon S3 bucket, see [Make SAP HANA software available for AWS Launch Wizard to deploy a HANA database](launch-wizard-sap-structure.md)\.
+ Depending on the operating system version you want to use for the SAP deployment, an SAP Marketplace subscription may be required\. For a complete list of supported operating system versions, see [Supported operating system versions for SAP deployments](launch-wizard-sap-versions.md#launch-wizard-sap-ascs-support-os)\.

## AWS Identity and Access Management \(IAM\)<a name="launch-wizard-sap-iam"></a>

Establishing the AWS Identity and Access Management \(IAM\) role and setting up the IAM user for permissions are typically performed by **an IAM administrator** for your organization\. The steps are as follows: 
+ A one\-time creation of IAM roles that Launch Wizard uses to deploy SAP systems on AWS\.
+ The creation of IAM users who can grant permission for Launch Wizard to deploy applications\.

**Topics**
+ [One\-time creation of IAM role](#launch-wizard-sap-iam-role)
+ [Create and enable IAM users to use Launch Wizard](#launch-wizard-iam-user-setup)
+ [Add permissions to use AWS KMS keys](#launch-wizard-sap-iam-encryption)
+ [Add permissions to run custom pre\- and post\-deployment configuration scripts](#launch-wizard-sap-iam-scripts)
+ [Add permissions to save deployment artifacts to Amazon S3](#launch-wizard-sap-iam-s3-artifacts)

### One\-time creation of IAM role<a name="launch-wizard-sap-iam-role"></a>

On the **Choose Application** page of Launch Wizard, under **Permissions**, Launch Wizard displays the IAM roles required for Launch Wizard to access other AWS services on your behalf\. These roles are **AmazonEC2RoleForLaunchWizard** and **AmazonLambdaRoleForLaunchWizard**\. Select **Next**, and Launch Wizard attempts to discover the IAM roles in your account\. If either role does not exist in your account, Launch Wizard attempts to create the roles with the same names, **AmazonEC2RoleForLaunchWizard** and **AmazonLambdaRoleForLaunchWizard**\. 

The **AmazonEC2RoleForLaunchWizard** role is comprised of two IAM managed policies: **AmazonSSMManagedInstanceCore** and **AmazonEC2RolePolicyForLaunchWizard**\. The **AmazonEC2RoleForLaunchWizard** role is used by the instance profile for the Amazon EC2 instances that Launch Wizard launches into your account as part of the deployment\. 

If you want to deploy AWS Backint Agent as a backup and restore solution for your application, you must attach a policy to the **AmazonEC2RoleForLaunchWizard** so that Launch Wizard can perform Backint Agent operations on your behalf\. The required policy and instructions can be found in [ Step 2 of the Backint Agent IAM documentation](https://docs.aws.amazon.com/sap/latest/sap-hana/aws-backint-agent-prerequisites.html#aws-backint-agent-iam)\. During a deployment, Launch Wizard provides the policy as well as the steps to update the role, taking user specifications into account\. 

The **AmazonLambdaRoleForLaunchWizard** is also comprised of two IAM managed policies: **AWSLambdaVPCAccessExecutionRole** and **AmazonLambdaRolePolicyForLaunchWizardSAP**\. The **AmazonLambdaRoleForLaunchWizard** role is used by the Lambda function invoked by the service in your account to perform certain deployment\-related actions, such as validation of route tables, and pre\-configuration and configuration tasks for HA mode enabling\. 

After the IAM roles are created, the IAM administrator can either continue with the deployment process or optionally delegate the application deployment process to another IAM user, as described in the following section\. At this point in the IAM set up process, the IAM administrator can exit the Launch Wizard service\. 

### Create and enable IAM users to use Launch Wizard<a name="launch-wizard-iam-user-setup"></a>

To deploy an SAP system with Launch Wizard, the user must be assigned the **AmazonLaunchWizard\_Fullaccess** policy\. The following steps guide the IAM administrator through the process of attaching an IAM policy to an IAM user to grant that user permission to access and deploy applications from Launch Wizard\. 

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam)\.

1. In the left navigation pane, choose **Policies**\.

1. Choose **Users**\.

1. Choose the name of the user whose permissions you want to modify, and then choose the **Permissions** tab\.

1. Choose **Add permissions**\.

1. Choose **Attach existing policies directly to user**\.

1. Search for the policy named **AmazonLaunchWizard\_Fullaccess** and select the check box to the left of the policy name\.

1. Choose **Next: Review** to see the list of policies to attach to the user\.

1. Verify that the correct policy is listed, and then choose **Add permissions**\.

**Important**  
You must log in with the user associated with this IAM policy when you use Launch Wizard\.

### Add permissions to use AWS KMS keys<a name="launch-wizard-sap-iam-encryption"></a>

AWS Launch Wizard uses AWS default encryption keys to encrypt Amazon EBS volumes\. In addition, Launch Wizard supports the use of KMS keys created and maintained in AWS KMS\. You can choose to either create new keys or use preexisting keys to encrypt your EBS volumes\. You must add permissions to the KMS key policy for your key so that Launch Wizard can use your KMS key for encryption\.

**How to add permissions to your KMS key policy so that Launch Wizard can use your key for encryption**

1. Sign in to the AWS Management Console and open the AWS Key Management Service \(AWS KMS\) console at [https://console\.aws\.amazon\.com/kms](https://console.aws.amazon.com/kms)\.

1. To change the AWS Region, use the Region selector in the upper\-right corner of the page\.

1. Choose **Customer managed keys** in the left navigation pane\.

1. Select the alias of the KMS key that you want to use to encrypt your EBS volumes\.

1. Under **Key users**, choose **Add**\.

1. Select the check box next to `AmazonEC2RoleForLaunchWizard` and the IAM user with Launch Wizard full access permissions\.

1. Choose **Add**\. Verify that `AmazonEC2RoleForLaunchWizard` and the IAM user with Launch Wizard full access permissions appear in the **Key users** list\.

### Add permissions to run custom pre\- and post\-deployment configuration scripts<a name="launch-wizard-sap-iam-scripts"></a>

To run custom pre\- and post\-configuration deployment scripts, you must add the following permissions to the `AmazonEC2RoleForLaunchWizard` role\. The following steps guide you through the process of adding the required permissions for using custom scripts to the `AmazonEC2RoleForLaunchWizard` role\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam)\.

1. In the left navigation pane, choose **Policies**\.

1. Choose **Users**\.

1. Choose the name of the user whose permissions you want to modify, and then choose the **Permissions** tab\.

1. Choose **Add inline policy**\.

1. Copy and paste the following policy into the **JSON** tab\. Enter the S3 paths where your scripts are stored\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "VisualEditor0",
               "Effect": "Allow",
               "Action": [
                   "s3:GetObject",
                   "s3:GetBucketLocation"
               ],
               "Resource": [
                   "arn:aws:s3:::<S3bucket1>/<S3prefix1>/<script1>",
                   "arn:aws:s3:::<S3bucket2>/<S3prefix2>/<script2>",
                   "arn:aws:s3:::<S3bucket1>",
                   "arn:aws:s3:::<S3bucket2>"
               ]
           }
       ]
   }
   ```

1. Choose **Review policy** and enter a name for the policy\.

1. Choose **Create Policy**\.

1. Verify that the correct policy is listed, and then choose **Policy actions**\.

1. Choose **Attach**\.

1. Search for the policy named **AmazonEC2RoleForLaunchWizard** and select the check box to the left of the policy name\.

1. Choose **Attach policy**\.

**Important**  
You must log in with the user associated with this IAM policy when you use Launch Wizard\.

If the pre\- or post\-deployment configuration deployment scripts are expected to run additional AWS services, the permissions to use the services must also be manually added as policy to the `AmazonEC2RoleForLaunchWizard`\.

### Add permissions to save deployment artifacts to Amazon S3<a name="launch-wizard-sap-iam-s3-artifacts"></a>

To create AWS Service Catalog products from successful deployments, which include AWS CloudFormation templates and application configuration scripts, you must provide access to an Amazon S3 location to save the generated artifacts\. 

The following steps guide you through adding the required permissions for saving deployment artifacts to Amazon S3 to the `AmazonLaunchWizard_Fullaccess` role\. If the S3 bucket that you want to use to save deployment artifacts does not contain the prefix `launchwizard` in its name, you must perform the following steps to attach the required policy to the IAM user who will be performing the deployments\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam)\. \.

1. In the left navigation pane, choose **Users**, and choose the IAM user to which you want to grant permissions\. By default, the following policy should be attached to the user: **AmazonLaunchWizard\_Fullaccess**

1. Choose the **Permissions** tab\.

1. Choose **Add inline policy**\.

1. Copy and paste the following policy into the **JSON** tab, replacing the placeholder text\. Enter the Amazon S3 path where you want to store your artifacts in the policy\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
         
         {
             "Sid": "SaveLaunchWizardDeploymentArtifacts",
             "Effect": "Allow",
             "Action": [
               "s3:PutObject"
             ],
             "Resource": [
                 "arn:aws:s3:::${bucketName}/${bucketFolder}*"
             ]
         }
       ]
     }
   ```

1. Choose **Review policy** and enter a **Name** for the policy\.

1. Choose **Create Policy**\.

1. Verify that the correct policy is listed for the user\.