# Troubleshoot AWS Launch Wizard for Exchange Server<a name="launch-wizard-exchange-troubleshooting"></a>

Each application in your account in the same AWS Region can be uniquely identified by the application name specified at the time of a deployment\. You can use the application name to view the details related to the application launch\.

For information about issues encountered after a successful deployment, see the [Troubleshooting](https://aws-quickstart.github.io/quickstart-microsoft-exchange/#_troubleshooting) section of the Exchange Server on the AWS Cloud Quick Start deployment guide\.

**Topics**
+ [Launch Wizard provisioning events](#launch-wizard-exchange-provisioning)
+ [AWS CloudFormation stack](#launch-wizard-exchange-cloudformation)
+ [Application launch quotas](#launch-wizard-exchange-quotas)
+ [Enable termination protection](#launch-wizard-exchange-terminate-protection)
+ [Errors](#launch-wizard-exchange-errors)

## Launch Wizard provisioning events<a name="launch-wizard-exchange-provisioning"></a>

Launch Wizard captures events from AWS CloudFormation to track the status of an ongoing application deployment\. If an application deployment fails, you can access the AWS Launch Wizard console to view the deployment events for this application by selecting **Deployments** from the navigation pane\. A failed event shows a status of **Failed** along with a failure message\. 

## AWS CloudFormation stack<a name="launch-wizard-exchange-cloudformation"></a>

Launch Wizard uses AWS CloudFormation to provision the infrastructure resources of an application\. You can view the status of these AWS CloudFormation stacks, and if any of the stacks fail, you can view the cause of the failure\. AWS CloudFormation stacks can be found in your account using the AWS CloudFormation [describe\-stacks](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-describing-stacks.html) API or by accessing the stack in the AWS CloudFormation console\. The following can be used with the `describe-stacks` API for the `--stack-name` argument:
+ **Application resources**

  `LaunchWizard-APPLICATION_NAME`\. This stack also has nested stacks for VPC, load balancer, and bastion hosts, among other components\.

## Application launch quotas<a name="launch-wizard-exchange-quotas"></a>

Launch Wizard allows three active applications with the status of `in progress` at one time\. The combined maximum amount of `in progress` and `completed` active applications is 25 for any given application type\. If you want to increase this limit, contact [AWS Support](https://aws.amazon.com/contact-us)\.

## Enable termination protection<a name="launch-wizard-exchange-terminate-protection"></a>

If you encounter errors when you deploy Exchange with Launch Wizard, and the log information provided by Launch Wizard or AWS CloudFormation is not sufficient to determine your issue, you must [connect to the instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html) within the Amazon EC2 Auto Scaling group to determine the cause of the failure\. When you connect to an instance to troubleshoot deployment failures, a common cause is the deployment scripts failing on the operating system\. The following error messages in AWS CloudFormation can indicate the deployment scripts failed:
+ 

  ```
  Received 1 FAILURE signal(s) out of 1. Unable to satisfy 100% MinSuccessfulInstancesPercent requirement
  ```
+ 

  ```
  WaitCondition received failed message: ‘Error: Failed in function <script function name>. Return code 1 , warnings: <any warnings>’ for uniqueId: <Resource/wait condition name>
  ```
+ 

  ```
  <Resource name> timed out. Failed to receive 1 resource signal(s) within the specified duration
  ```
+ 

  ```
  Unparsable WaitCondition data
  ```

 You can connect to an EC2 instance only if it is not terminated\. Launch Wizard terminates instances on stack creation failure by default\. You can enable the **Deactivate rollback on failed deployment** setting during deployment to prevent this behavior\. If the setting was not enabled, you can still prevent the instance from getting terminated by updating the termination settings of that instance from the EC2 console before the AWS CloudFormation stack gets rolled back\.

**Note**  
When you enable **Deactivate rollback on failed deployment**, you continue to incur AWS charges for the stack\. Ensure that you delete the stack when you finish troubleshooting\.

**To find the EC2 instances from the Launch Wizard deployment**

1. Access the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Choose the AWS CloudFormation stack of the Launch Wizard deployment, and choose the **Resources tab**\.

1. Choose the resource with type **AWS::AutoScaling::AutoScalingGroup**\.

1. Select the **instance management** tab\. This page will have a link to the EC2 console, which lists the instances in the Launch Wizard deployment\.

You can update the termination settings to disable termination of the instances from the EC2 console\. From the **Instances** page, select an instance and choose **Action** > **Instance Settings** > **Change Termination Protection**\. Then choose **Yes, Enable**\.

After you have determined the root cause, disable the termination protection before you delete the deployment in Launch Wizard\.

## Errors<a name="launch-wizard-exchange-errors"></a>

**Your requested instance type is not supported in your requested Availability Zone**
+ **Cause:** This failure might occur while launching instances for the Launch Wizard deployment\.
+ **Solution:** You must choose a different Availability Zone and retry the deployment from the initial page of the Launch Wizard console\.

**EC2 instance stabilization error**
+ **Cause:** Failure can occur if an EC2 instance fails to stabilize\. When this happens, the EC2 instance is unable to communicate to the AWS CloudFormation service to signal completions, resulting in `WaitCondition` errors\.
+ **Solution:** `WaitCondition` errors are often transient EC2 failures and retrying the deployment may succeed\. For additional assistance, contact [AWS Support](https://aws.amazon.com/contact-us)\.

**Permission errors**
+ **Cause:** Insufficient AWS Identity and Access Management \(IAM\) permissions could be the cause of various failures in the Launch Wizard deployment\. Errors caused by insufficient permissions may occur within the EC2 instances as scripts are run during the application deployment\. Other errors may return a verbose message indicating there are insufficient permissions similar to the following:

  ```
  User: arn:aws:iam::123456789098:user/test-user is not authorized to perform: elasticloadbalancing:CreateTargetGroup on resource: arn:aws:elasticloadbalancing:us-east-1:123456789098:targetgroup/myTargetGroup/*)
  ```
+ **Solution:** Before deploying the Launch Wizard application, you must sign in to the AWS Management Console with IAM permissions for the resources that Launch Wizard will deploy\. The *AdministratorAccess* managed policy within IAM provides sufficient permissions, although your organization may choose to use a custom policy with more restrictions\.