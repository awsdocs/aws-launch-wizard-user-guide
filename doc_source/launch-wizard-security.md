# AWS Launch Wizard security<a name="launch-wizard-security"></a>

Cloud security at AWS is the highest priority\. As an AWS customer, you benefit from a data center and network architecture that is built to meet the requirements of the most security\-sensitive organizations\.

Security is a shared responsibility between AWS and you\. The [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) describes this as security *of* the cloud and security *in* the cloud:
+ **Security of the cloud** – AWS is responsible for protecting the infrastructure that runs AWS services in the AWS Cloud\. AWS also provides you with services that you can use securely\. Third\-party auditors regularly test and verify the effectiveness of our security as part of the [AWS Compliance Programs](http://aws.amazon.com/compliance/programs/)\. To learn about the compliance programs that apply to AWS Launch Wizard, see [AWS Services in Scope by Compliance Program](http://aws.amazon.com/compliance/services-in-scope/)\.
+ **Security in the cloud** – Your responsibility is determined by the AWS service that you use\. You are also responsible for other factors including the sensitivity of your data, your company’s requirements, and applicable laws and regulations\. 

This documentation helps you understand how to apply the shared responsibility model when using AWS Launch Wizard\. The following topics show you how to configure Launch Wizard to meet your security and compliance objectives\. You also learn how to use other AWS services that help you to monitor and secure your Launch Wizard resources\. 

AWS Launch Wizard deploys Amazon EC2 instances into Amazon VPCs\. For security information for Amazon EC2 and Amazon VPC, see the security sections in the [Amazon EC2 Getting Started Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_Network_and_Security.html) and the [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html)\.

This section of the Launch Wizard User Guide provides security information that pertains to AWS Launch Wizard\. For security topics specific to AWS Launch Wizard for SQL Server, see [Security groups and firewalls](launch-wizard-best-practices.md#launch-wizard-sql-security)\. For security topics specific to AWS Launch Wizard for SAP, see [Security groups in AWS Launch Wizard for SAP](launch-wizard-sap-security-groups.md)\. 

**Topics**
+ [Infrastructure security in Launch Wizard](#infrastructure-security)
+ [Resilience in Launch Wizard](#disaster-recovery-resiliency)
+ [Data protection in Launch Wizard](#data-protection)
+ [Identity and Access Management for AWS Launch Wizard](#identity-access-management)
+ [Update management in Launch Wizard](#update-management)

## Infrastructure security in Launch Wizard<a name="infrastructure-security"></a>

As a managed service, Launch Wizard is protected by the AWS global network security procedures that are described in the [Amazon Web Services: Overview of Security Processes](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf) whitepaper\.

## Resilience in Launch Wizard<a name="disaster-recovery-resiliency"></a>

The AWS global infrastructure is built around AWS Regions and Availability Zones\. Regions provide multiple physically separated and isolated Availability Zones, which are connected through low\-latency, high\-throughput, and highly redundant networking\. With Availability Zones, you can design and operate applications and databases that automatically fail over between Availability Zones without interruption\. Availability Zones are more highly available, fault tolerant, and scalable than traditional single or multiple data center infrastructures\.

For more information about AWS Regions and Availability Zones, see [AWS Global Infrastructure](http://aws.amazon.com/about-aws/global-infrastructure/)\.

AWS Launch Wizard sets up an application across multiple Availability Zones to ensure automatic failover between Availability Zones without interruption\. Availability Zones are more highly available, fault tolerant, and scalable than traditional single or multiple datacenter infrastructures\. 

## Data protection in Launch Wizard<a name="data-protection"></a>

The AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) applies to data protection in AWS Launch Wizard\. As described in this model, AWS is responsible for protecting the global infrastructure that runs all of the AWS Cloud\. You are responsible for maintaining control over your content that is hosted on this infrastructure\. This content includes the security configuration and management tasks for the AWS services that you use\. For more information about data privacy, see the [Data Privacy FAQ](http://aws.amazon.com/compliance/data-privacy-faq)\. For information about data protection in Europe, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

For data protection purposes, we recommend that you protect AWS account credentials and set up individual user accounts with AWS Identity and Access Management \(IAM\)\. That way each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\. We recommend TLS 1\.2 or later\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls within AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing personal data that is stored in Amazon S3\.
+ If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

We strongly recommend that you never put sensitive identifying information, such as your customers' account numbers, into free\-form fields such as a **Name** field\. This includes when you work with Launch Wizard or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into Launch Wizard or other services might get picked up for inclusion in diagnostic logs\. When you provide a URL to an external server, don't include credentials information in the URL to validate your request to that server\.

## Identity and Access Management for AWS Launch Wizard<a name="identity-access-management"></a>

AWS Launch Wizard uses the following AWS managed policies to grant permissions to users and services\.
+ ** AmazonEC2RolePolicyForLaunchWizard**

  AWS Launch Wizard creates an IAM role with the name **AmazonEC2RoleForLaunchWizard ** in your account if the role already does not already exist in your account\. If the role exists, the role is attached to the instance profile for the Amazon EC2 instances that Launch Wizard will launch into your account\. This role is comprised of two IAM managed policies: **AmazonSSMManagedInstanceCore** and **AmazonEC2RolePolicyForLaunchWizard**\.

  When you choose to deploy your SAP application with AWS Backint Agent for SAP HANA, you must attach the IAM inline policy provided in [ Step 2 of the AWS Identity and Access Management documentation for AWS Backint Agent for SAP HANA](https://docs.aws.amazon.com/sap/latest/sap-hana/aws-backint-agent-prerequisites.html#aws-backint-agent-iam)\. This policy and instructions to attach the policy to the role are provided by Launch Wizard\.
+ **AmazonSSMManagedInstanceCore**

   This policy enables AWS Systems Manager service core functionality on Amazon EC2\. For information, see [Create an IAM Instance Profile for Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/setup-instance-profile.html)\.
+ **AmazonLaunchWizard\_Fullaccess**

  This policy provides full access to AWS Launch Wizard and other required services\. 
+ ** AWSLambdaVPCAccessExecutionRole**

  This policy provides minimum permissions for a Lambda function to execute while accessing a resource within a VPC\. These permissions include create, describe, delete network interfaces, and write permissions to CloudWatch Logs\.
+ **AmazonLambdaRolePolicyForLaunchWizardSAP**

  This policy provides minimum permissions to enable SAP provisioning scenarios on Launch Wizard\. It allows invocation of Lambda functions to be able to perform certain actions, such as validation of route tables and perform pre\-configuration and configuration tasks for HA mode enabling\.
+ To run custom pre\- and post\-configuration deployment scripts, you must manually add the permissions provided in [Add permissions to run custom pre\- and post\-deployment configuration scripts](launch-wizard-sap-setting-up.md#launch-wizard-sap-iam-scripts) to the `AmazonEC2RoleForLaunchWizard` role\.

If you deploy domain controllers into an existing VPC with an existing Active Directory, Launch Wizard for Active Directory requires domain administrator credentials to be added to Secrets Manager in order to join your domain controllers to Active Directory and promote them\. In addition, the following resource policy must be attached to the secret so that Launch Wizard can access the secret\. Launch Wizard guides you through the process of attaching the required policy to your secret\.

```
{
    "Version" : "2012-10-17",
    "Statement" : [ {
        "Effect" : "Allow",
        "Principal" : {
            "AWS" :
            "arn:aws:iam::<account-id>:role/service-role/AmazonEC2RoleForLaunchWizard"
        },
        "Action" : [
            "secretsmanager:GetSecretValue",
            "secretsmanager:CreateSecret",
            "secretsmanager:GetRandomPassword"
        ],
        "Resource" : "*"
        } 
    ]
}
```

## Update management in Launch Wizard<a name="update-management"></a>

We recommend that you regularly patch, update, and secure the operating system and applications on your EC2 instances\. You can use [AWS Systems Manager Patch Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-patch.html) to automate the process of installing security\-related updates for both the operating system and applications\. Alternatively, you can use any automatic update services or recommended processes for installing updates that are provided by the application vendor\.
