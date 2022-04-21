# Troubleshoot AWS Launch Wizard for SAP<a name="launch-wizard-sap-troubleshooting"></a>

Each application in your account in the same AWS Region can be uniquely identified by the application name specified at the time of a deployment\. The application name can be used to view the details related to the application launch\.

**Topics**
+ [Launch Wizard provisioning events](#launch-wizard-sap-provisioning)
+ [CloudWatch Logs](#launch-wizard-sap-logs)
+ [AWS CloudFormation stack](#launch-wizard-sap-cloudformation)
+ [Pre\- and post\-deployment configuration scripts](#launch-wizard-sap-troubleshooting-scripts)
+ [Application launch quotas](#launch-wizard-sap-quotas)
+ [Enable termination protection](#launch-wizard-sap-terminate-protection)
+ [Instance level logs](#launch-wizard-sap-instance-level-logs)
+ [SAP application software deployment logs](#launch-wizard-sap-application-logs)
+ [Errors](#launch-wizard-sap-errors)

## Launch Wizard provisioning events<a name="launch-wizard-sap-provisioning"></a>

Launch Wizard captures events from SSM Automation and AWS CloudFormation to track the status of an ongoing application deployment\. If an application deployment fails, you can view the deployment events for this application by selecting **Deployments** from the navigation pane\. A failed event shows a status of **Failed** along with a failure message\. 

## CloudWatch Logs<a name="launch-wizard-sap-logs"></a>

Launch Wizard streams provisioning logs from all of the AWS log sources, such as AWS CloudFormation, SSM, and CloudWatch Logs\. CloudWatch Logs for a given application name can be viewed on the CloudWatch console for the log group name `LaunchWizard-APPLICATION_NAME` and log stream `ApplicationLaunchLog`\. 

## AWS CloudFormation stack<a name="launch-wizard-sap-cloudformation"></a>

Launch Wizard uses AWS CloudFormation to provision the infrastructure resources of an application\. AWS CloudFormation stacks can be found in your account using the AWS CloudFormation [describe\-stacks](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-describing-stacks.html) API\. Launch Wizard launches various stacks in your account for validation and application resource creation\. The following are the relevant filters for the [describe\-stacks](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-describing-stacks.html) API\.
+ **Application resources**

  `LaunchWizard-APPLICATION_NAME`\. 

You can view the status of these AWS CloudFormation stacks\. If any of them fail, you can view the cause of the failure\.

## Pre\- and post\-deployment configuration scripts<a name="launch-wizard-sap-troubleshooting-scripts"></a>

**Can't find the output of my scripts**
+ **Cause:** Customizations are key scripts that you want to run on the EC2 instances and the logs from script deployments are not included with the provisioning logs\. 
+ **Solution:** The logs for scripts that run on EC2 instances are included in the CloudWatch log group that Launch Wizard creates in your account for the workload\. The CloudWatch log group can be identified as `LaunchWizard-APPLICATION_NAME` \. You can find the following logs in this log group\.
  + `lw-customization/<instance-id>/preDeploymentConfiguration` — For pre\-deployment configuration scripts that run on the specified EC2 instance\.
  + `lw-customization/<instance-id>/postDeploymentConfiguration` — For post\-deployment configuration scripts that run on the specified EC2 instance\.

## Application launch quotas<a name="launch-wizard-sap-quotas"></a>

Launch Wizard allows for a maximum of 25 active applications for any given application type\. Up to three applications can be `in progress` at a time\. If you want to increase this limit, contact [AWS Support](https://aws.amazon.com/contact-us)\.

## Enable termination protection<a name="launch-wizard-sap-terminate-protection"></a>

If you encounter errors when you deploy SAP systems with Launch Wizard, and the log information provided by Amazon CloudWatch is not sufficient to determine your issue, you must log in to the instance to determine the root cause\. In case of failures, Launch Wizard terminates the instances, which limits the means to troubleshoot a failure\. 

You can update the termination settings to disable termination of the instances from the EC2 console\. From the **Instances** page, select an instance and choose **Action** > **Instance Settings** > **Change Termination Protection**\. Then choose **Yes, Enable**\.

After you have determined the root cause, disable the termination protection before you delete the deployment in Launch Wizard\.

## Instance level logs<a name="launch-wizard-sap-instance-level-logs"></a>

Detailed deployment and configuration logs can be found in `/root/install/scripts/log` during the deployment process, when the instances are being launched and configured\. The name of the log file is `install.log`\. To check the progress of the deployment, you can log in to an instance as soon its instance state is listed as **running**\.

When the deployment is finished, the log files are moved to `/tmp`\.

In addition to the `install.log` file, the `install.dbg` file includes additional error information in case of failure scenarios\. `install.json` includes all of your console selections to configure the deployment\.

## SAP application software deployment logs<a name="launch-wizard-sap-application-logs"></a>

Depending on which SAP components are deployed on an instance, Launch Wizard creates a folder in `/tmp` to log all of the SAP software application deployment logs\. If a database component is deployed on an instance, the folder name in the file will be `NW_ABAP_DB`\. If an application server is deployed, the folder name will be `NW_ABAP_APP`\. For single node deployments, there will be multiple folders, such as `NW_ABAP_DB` and `NW_ABAP_CI`, which represent the different components deployed on the instance\.

## Errors<a name="launch-wizard-sap-errors"></a>

**Your requested instance type is not supported in your requested Availability Zone**
+ **Cause:** This failure might occur during the launch of your instance, or during the validation of the instances that Launch Wizard launches in your selected subnets\. 
+ **Solution:** For this scenario, you must choose a different Availability Zone and retry the deployment from the initial page of the Launch Wizard console\.

**Infrastructure template already exists**
+ **Cause:** This failure occurs when you choose to create a new infrastructure configuration and then navigate back to the first step in the wizard to review or adjust any settings\. Launch Wizard has already registered the configuration template, so choosing **Next** results in the error "Template name already exists\. Select a new template name\." 
+ **Solution:** 

  Perform one of the following actions to continue with your deployment\.
  + Change the name of the configuration template and continue\.
  + Choose another template and continue\.
  + Delete the template causing the error by navigating to the **Saved Infrastructure Setting** tab under **Deployments – SAP**, and then continue with your configuration using the same configuration name\.