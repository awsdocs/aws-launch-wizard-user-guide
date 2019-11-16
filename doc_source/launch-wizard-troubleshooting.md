# Troubleshooting AWS Launch Wizard<a name="launch-wizard-troubleshooting"></a>

Each application in your account in the same AWS Region can be uniquely identified by the application name specified at the time of a deployment\. The application name can be used to view the details related to the application launch\.

**Topics**
+ [Launch Wizard Provisioning Events](#launch-wizard-provisioning)
+ [CloudWatch Logs](#launch-wizard-logs)
+ [SSM Automation Execution](#launch-wizard-ssm-automation)
+ [AWS CloudFormation Stack](#launch-wizard-cloudformation)
+ [Application Launch Limits](#launch-wizard-limits)
+ [Errors](#launch-wizard-errors)

## Launch Wizard Provisioning Events<a name="launch-wizard-provisioning"></a>

Launch Wizard captures events from SSM Automation and AWS CloudFormation to track the status of an ongoing application deployment\. If an application deployment fails, you can view the deployment events for this application by selecting **Deployments** from the navigation pane\. A failed event shows a status of **Failed** along with a failure message\. 

## CloudWatch Logs<a name="launch-wizard-logs"></a>

Launch Wizard streams provisioning logs from all of the AWS log sources, such as AWS CloudFormation, SSM, and CloudWatch Logs\. CloudWatch Logs for a given application name can be viewed on the CloudWatch console for the log group name `LaunchWizard-APPLICATION_NAME` and log stream `ApplicationLaunchLog`\. 

## SSM Automation Execution<a name="launch-wizard-ssm-automation"></a>

Launch Wizard uses SSM Automation to provision SQL Server Always On applications\. SSM Automation execution can be found in your account using the `ssm describe-automation-executions` API, and adding document name prefix filters\. Launch Wizard launches various automation documents in your account for validation and application provisioning\. The following are the relevant filters for the `ssm describe-automation-executions` API\.
+ **Validation: Validate VPC connectivity**

  `LaunchWizard-Validate-VPC-Connectivity-APPLICATION_NAME-Subnet_id`, where `Subnet_id` is the subnet on which to perform the validation\.
+ **Validation: Validate credentials**

  `LaunchWizard-Validate-Credentials-APPLICATION_NAME`
+ **Application Provisioning: Provisioning resources and Post Configuration**

  `LaunchWizard-SQLHAAlwaysOn-APPLICATION_NAME-Provision`

You can view the status of these SSM Automation executions\. If any of them fail, you can view the cause of the failure\.

## AWS CloudFormation Stack<a name="launch-wizard-cloudformation"></a>

Launch Wizard uses AWS CloudFormation to provision the infrastructure resources of an application\. CloudFormation stacks can be found in your account using the CloudFormation `describe-stacks` API\. Launch Wizard launches various stacks in your account for validation and application resource creation\. The following are the relevant filters for the `describe-stacks` API\.
+ **Validation**

  `LaunchWizard-APPLICATION_NAME-checkCredentials-SSM_execution_id`
+ **Validation**

  `LaunchWizard-APPLICATION_NAME-checkVPCConnectivity-SSM_execution_id`
+ **Application resources**

  `LaunchWizard-APPLICATION_NAME`\. This stack also has nested stacks for VPC, AD, the RDGW node, and SQL nodes\.

You can view the status of these CloudFormation stacks\. If any of them fail, you can view the cause of failure\.

## Application Launch Limits<a name="launch-wizard-limits"></a>

Launch Wizard allows for a maximum of 50 active applications \(with status `in progress` or `completed`\) for any given application type\. If you want to increase this limit, contact [AWS Support](https://aws.amazon.com/contact-us)\. Launch Wizard supports 3 paralell in\-progress deployment per account\. 

## Errors<a name="launch-wizard-errors"></a>

**Directory fails to create**
+ **Cause:** An internal service error has been encountered during the creation of the directory\.
+ **Solution:** Retry the operation\. For this scenario, you must retry the deployment from the initial page of the Launch Wizard console\.

**Your requested instance type is not supported in your requested Availability Zone**
+ **Cause:** This failure might happen during the launch of either your RDGW instance or your SQL Server instance, or during the validation of the instances that Launch Wizard launches in your selected subnets\. 
+ **Solution:** For this scenario, you must choose a different Availability Zone and retry the deployment from the initial page of the Launch Wizard console\.
