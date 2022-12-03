# Set up for AWS Launch Wizard for Active Directory<a name="launch-wizard-ad-setting-up"></a>

Verify the following prerequisites to deploy self\-managed domain controllers with AWS Launch Wizard\.

**Topics**
+ [Specialized knowledge](#launch-wizard-ad-specialized-knowledge)
+ [Amazon Web Services account](#launch-wizard-ad-aws-account)
+ [Technical requirements](#launch-wizard-ad-technical-requirements)
+ [Service Quotas](#launch-wizard-ad-resource-quotas)
+ [IAM permissions](#launch-wizard-ad-iam-permissions)
+ [Active Directory deployment options](#launch-wizard-ad-setup)

## Specialized knowledge<a name="launch-wizard-ad-specialized-knowledge"></a>

This deployment requires a moderate level of familiarity with AWS services\. If you’re new to AWS, see [Getting Started Resource Center](http://aws.amazon.com/getting-started) and [AWS Training and Certification](http://aws.amazon.com/training)\. These sites provide materials for learning how to design, deploy, and operate your infrastructure and applications on the AWS Cloud\.

This Launch Wizard deployment assumes familiarity with Active Directory concepts and usage\.

## Amazon Web Services account<a name="launch-wizard-ad-aws-account"></a>

If you don’t already have an AWS account, create one at [http://aws.amazon.com/](http://aws.amazon.com/) by following the on\-screen instructions\. Part of the sign\-up process involves receiving a phone call and entering a PIN using the phone keypad\.

Your AWS account is automatically signed up for all AWS services\. You are charged only for the services you use\. 

## Technical requirements<a name="launch-wizard-ad-technical-requirements"></a>

Before you start the Launch Wizard deployment, review the following information and make sure that your account is properly configured\. Otherwise, deployment might fail\. 

## Service Quotas<a name="launch-wizard-ad-resource-quotas"></a>

If necessary, [request service quota increases](https://console.aws.amazon.com/servicequotas/) for the resources deployed by Launch Wizard\. You might need to request increases if your existing deployment currently uses these resources and if this Launch Wizard deployment could result in exceeding the default quotas\. The [Service Quotas console](https://console.aws.amazon.com/servicequotas/) displays your usage and quotas for some aspects of some services\. For more information, see [What is Service Quotas?](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html) and [AWS service quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\.

## IAM permissions<a name="launch-wizard-ad-iam-permissions"></a>

Before deploying the Launch Wizard application, you must sign in to the AWS Management Console with IAM permissions for the resources that the templates deploy\. The *AdministratorAccess* managed policy within IAM provides sufficient permissions, although your organization may choose to use a custom policy with more restrictions\. For more information, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html)\.

## Active Directory deployment options<a name="launch-wizard-ad-setup"></a>

This section contains information on what configuration is performed for deployment of domain controllers into a new or existing VPC\. You can deploy a new Active Directory infrastructure on Amazon EC2, deploy a new AWS Managed Microsoft AD, or extend an existing on\-premises Active Directory into the AWS Cloud\.

### Active Directory configurations<a name="launch-wizard-ad-setup-managed"></a>

When you use Launch Wizard to deploy Active Directory, the following key operations are performed\. These operations result in the creation of new records or entries in Active Directory\.
+ When you create a new Active Directory domain, Launch Wizard creates two new Amazon EC2 instances and promotes the servers to domain controllers in your domain\.
+ When you extend an existing Active Directory domain, Launch Wizard creates two new Amazon EC2 instances and optionally joins them to the domain\.
+ When you create an AWS Managed Microsoft AD, Launch Wizard deploys the managed directory\.
+ All deployment types create ingress and egress rules to communicate with your domain controllers\.

### On\-premises Active Directory through AWS Direct Connect<a name="launch-wizard-ad-setup-extend"></a>

If you are deploying domain controllers to extend an on\-premises Active Directory into an existing VPC, ensure that the following prerequisites are in place\.
+ Make sure that you have connectivity between your AWS account and your on\-premises network\. You can establish a dedicated network connection from your on\-premises network to your AWS account with AWS Direct Connect\. For more information, see [the AWS Direct Connect documentation](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html)\. 
+ The domain functional level of your Active Directory domain controller must be Windows Server 2012 or later\.
+ The IP addresses of your DNS server must be either in the same VPC CIDR range as the one in which your Launch Wizard domain controllers will be created, or in the private IP address range\. 
+ The firewall on the Active Directory domain controllers should allow the connections from the Amazon VPC from which you will create the Launch Wizard deployment\. At a minimum, your configuration should include the ports mentioned in [How to configure a firewall for Active Directory domains and trusts](https://support.microsoft.com/en-us/help/179442/how-to-configure-a-firewall-for-domains-and-trusts)\.

You can optionally perform the following step\.
+ Establish DNS resolution across your environments\. For options on how to set this up, see [ How to Set Up DNS Resolution Between On\-Premises Networks and AWS using AWS Directory Service and Amazon Route 53](https://aws.amazon.com/blogs/security/how-to-set-up-dns-resolution-between-on-premises-networks-and-aws-using-aws-directory-service-and-amazon-route-53/) or [How to Set Up DNS Resolution Between On\-Premises Networks and AWS Using AWS Directory Service and Microsoft Active Directory](https://aws.amazon.com/blogs/security/how-to-set-up-dns-resolution-between-on-premises-networks-and-aws-using-aws-directory-service-and-microsoft-active-directory/)\.