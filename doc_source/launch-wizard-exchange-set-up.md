# Set up<a name="launch-wizard-exchange-set-up"></a>

**Topics**
+ [Specialized knowledge](#launch-wizard-exchange-specialized-knowledge)
+ [Amazon Web Services account](#launch-wizard-exchange-aws-account)
+ [Technical requirements](#launch-wizard-exchange-technical-requirements)
+ [Service Quotas](#launch-wizard-exchange-resource-quotas)
+ [IAM permissions](#launch-wizard-exchange-iam-permissions)

## Specialized knowledge<a name="launch-wizard-exchange-specialized-knowledge"></a>

This deployment requires a moderate level of familiarity with AWS services\. If you’re new to AWS, see [Getting Started Resource Center](http://aws.amazon.com/getting-started) and [AWS Training and Certification](http://aws.amazon.com/training)\. These sites provide materials for learning how to design, deploy, and operate your infrastructure and applications on the AWS Cloud\.

This Launch Wizard deployment assumes familiarity with Exchange Server concepts and usage\.

## Amazon Web Services account<a name="launch-wizard-exchange-aws-account"></a>

If you don’t already have an AWS account, create one at [http://aws.amazon.com/](http://aws.amazon.com/) by following the on\-screen instructions\. Part of the sign\-up process involves receiving a phone call and entering a PIN using the phone keypad\.

Your AWS account is automatically signed up for all AWS services\. You are charged only for the services you use\. 

## Technical requirements<a name="launch-wizard-exchange-technical-requirements"></a>

Before you start the Launch Wizard deployment, review the following information and make sure that your account is properly configured\. Otherwise, deployment might fail\. 

## Service Quotas<a name="launch-wizard-exchange-resource-quotas"></a>

If necessary, [request service quota increases](https://console.aws.amazon.com/servicequotas/) for the resources deployed by Launch Wizard\. You might need to request increases if your existing deployment currently uses these resources and if this Launch Wizard deployment could result in exceeding the default quotas\. The [Service Quotas console](https://console.aws.amazon.com/servicequotas/) displays your usage and quotas for some aspects of some services\. For more information, see [What is Service Quotas?](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html) and [AWS service quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\.

## IAM permissions<a name="launch-wizard-exchange-iam-permissions"></a>

Before deploying the Launch Wizard application, you must sign in to the AWS Management Console with IAM permissions for the resources that the templates deploy\. The *AdministratorAccess* managed policy within IAM provides sufficient permissions, although your organization may choose to use a custom policy with more restrictions\. For more information, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html)\.