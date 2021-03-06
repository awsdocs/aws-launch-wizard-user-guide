# Troubleshoot AWS Launch Wizard for Active Directory<a name="launch-wizard-ad-troubleshooting"></a>

Each deployment in your account in the same AWS Region can be uniquely identified by the name specified at the time of a deployment\. The deployment name can be used to view the details related to the deployment on the **Deployments** page of the Launch Wizard console\.

This section describes steps to help you troubleshoot deploying domain controllers with Launch Wizard for Active Directory\.

**Topics**
+ [Launch Wizard provisioning events](#launch-wizard-ad-provisioning)
+ [CloudWatch Logs](#launch-wizard-ad-logs)
+ [AWS CloudFormation stack](#launch-wizard-ad-cloudformation)
+ [Errors](#launch-wizard-ad-errors)

## Launch Wizard provisioning events<a name="launch-wizard-ad-provisioning"></a>

Launch Wizard captures events from AWS CloudFormation to track the status of an ongoing application deployment\. If an application deployment fails, you can view the deployment events for this application by selecting **Deployments** from the navigation pane\. A failed event shows a status of **Failed** along with a failure message\. 

## CloudWatch Logs<a name="launch-wizard-ad-logs"></a>

Launch Wizard streams provisioning logs from all of the AWS log sources, such as AWS CloudFormation and PowerShell DSC scripts to CloudWatch Logs\. You can view the CloudWatch Logs for a given application name on the CloudWatch console for the log group name `LaunchWizard-APPLICATION_NAME` and log stream `ApplicationLaunchLog`\. 

## AWS CloudFormation stack<a name="launch-wizard-ad-cloudformation"></a>

Launch Wizard uses AWS CloudFormation to provision the infrastructure resources of an application\. CloudFormation stacks can be found in your account using the CloudFormation `describe-stacks` API\. Launch Wizard launches various stacks in your account for validation and application resource creation\. The following are the relevant filters for the `describe-stacks` API\.
+ Application Resources 
  + `LaunchWizard-APPLICATION_NAME`\. This stack includes all of the resource creation events for resources created by the deployment\.
  + `LaunchWizard-STACK_NAME-TEMPLATE_NAME`\. This log includes all of the logs from each PowerShell script run from within the instance\.

You can view the status of these CloudFormation stacks\. If any of them fail, you can view the cause of failure\.

## Errors<a name="launch-wizard-ad-errors"></a>

**Failed to create Forest Trust**
+ **Cause:** Forest trust fails because of the lack of connectivity between the two domain controllers\.
+ **Solution:** Ensure connectivity between the VPCs and ensure the ports are open between them\. See [Configure forest trust relationships](launch-wizard-ad-create-trusts.md) for more details about configuring forest trusts\.

**The requested instance type is not supported in the requested Availability Zone**
+ **Cause:** This failure can occur when you launch either your RDGW instance or your Active Directory Server instance, or during the validation of the instances that Launch Wizard launches in your selected subnets\. 
+ **Solution:** Choose a different Availability Zone and retry the deployment from the initial page of the Launch Wizard console\.

**Instance stabilization error**
+ **Cause:** This failure can occur when the EC2 instance used for validation fails to stabilize\. When this happens, the EC2 instance is unable to communicate to the CloudFormation service to signal completions, resulting in `WaitCondition` errors\.
+ **Solution:** Please contact [AWS Support](https://console.aws.amazon.com/support)\.
