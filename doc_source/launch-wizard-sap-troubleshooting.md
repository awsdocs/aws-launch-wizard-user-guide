# Troubleshoot AWS Launch Wizard for SAP<a name="launch-wizard-sap-troubleshooting"></a>

Each application in your account in the same AWS Region can be uniquely identified by the application name specified at the time of a deployment\. The application name can be used to view the details related to the application launch\.

**Topics**
+ [Launch Wizard provisioning events](#launch-wizard-sap-provisioning)
+ [CloudWatch Logs](#launch-wizard-sap-logs)
+ [AWS CloudFormation stack](#launch-wizard-sap-cloudformation)
+ [Pre\- and post\-deployment configuration scripts](#launch-wizard-sap-troubleshooting-scripts)
+ [Application launch quotas](#launch-wizard-sap-quotas)
+ [Instance level logs](#launch-wizard-sap-instance-level-logs)
+ [SAP application software deployment logs](#launch-wizard-sap-application-logs)
+ [Errors](#launch-wizard-sap-errors)
+ [Support](#launch-wizard-sap-support)

## Launch Wizard provisioning events<a name="launch-wizard-sap-provisioning"></a>

Launch Wizard captures events from SSM Automation and AWS CloudFormation to track the status of an ongoing application deployment\. If an application deployment fails, you can view the deployment events for this application by selecting **Deployments** from the navigation pane\. A failed event shows a status of **Failed** along with a failure message\. 

## CloudWatch Logs<a name="launch-wizard-sap-logs"></a>

Launch Wizard streams provisioning logs from all of the AWS log sources, such as AWS CloudFormation, SSM, and CloudWatch Logs\. You can access CloudWatch logs for your SAP deployment with the following steps\.

1. Sign in to console\.aws\.amazon\.com and go to AWS Launch Wizard\.

1. Under **Deployments** on the left panel, go to **SAP** and you can see the list of your SAP deployments\.

1. Select the failed deployment for which you want to verify the logs\.

1. Choose **Actions** > **View/Manage resources** > **View CloudWatch application logs**\.

1. You can now view the detailed logs and log streams that provide additional information on the SAP application type that failed during deployment\.

## AWS CloudFormation stack<a name="launch-wizard-sap-cloudformation"></a>

Launch Wizard uses AWS CloudFormation to provision the infrastructure resources of an application\. Launch Wizard launches various stacks in your account for validation and application resource creation\. You can verify the stacks via AWS console or AWS CLI\.

------
#### [ Console ]

1. Sign in to console\.aws\.amazon\.com and go to AWS Launch Wizard\.

1. Under **Deployments** on the left panel, go to **SAP** and you can see the list of your SAP deployments\.

1. Select the failed deployment for which you want to verify the stacks\.

1. Choose **Actions** > **View/Manage resources** > **View CloudFormation template **\.

1. You can now view all the stacks and their current status\. To see more details on any stack, select a **Stack name**\.

1. You are now on the **Stack details** page of your selected stack\. Choose **Events** from the top menu bar to view the cause of the failure\.

------
#### [ CLI ]

 AWS CloudFormation stacks can be found in your account using the AWS CloudFormation [describe\-stacks](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-describing-stacks.html) API\. The following are the relevant filters for the [describe\-stacks](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-describing-stacks.html) API\.
+ **Application resources**

  `LaunchWizard-APPLICATION_NAME`\. 

You can view the status of these AWS CloudFormation stacks\. If any of them fail, you can view the cause of the failure\.

------

## Pre\- and post\-deployment configuration scripts<a name="launch-wizard-sap-troubleshooting-scripts"></a>

**Can't find the output of my scripts**
+ **Cause:** Customizations are key scripts that you want to run on the EC2 instances and the logs from script deployments are not included with the provisioning logs\. 
+ **Solution:** The logs for scripts that run on EC2 instances are included in the CloudWatch log group that Launch Wizard creates in your account for the workload\. The CloudWatch log group can be identified as `LaunchWizard-APPLICATION_NAME` \. You can find the following logs in this log group\.
  + `lw-customization/<instance-id>/preDeploymentConfiguration` — For pre\-deployment configuration scripts that run on the specified EC2 instance\.
  + `lw-customization/<instance-id>/postDeploymentConfiguration` — For post\-deployment configuration scripts that run on the specified EC2 instance\.

## Application launch quotas<a name="launch-wizard-sap-quotas"></a>

Launch Wizard allows for a maximum of 25 active applications for any given application type\. Up to three applications can be `in progress` at a time\. If you want to increase this limit, contact [AWS Support](https://aws.amazon.com/contact-us)\.

## Instance level logs<a name="launch-wizard-sap-instance-level-logs"></a>

To check the progress of a deployment, you can log in to an instance as soon its instance state is listed as **running**\. When the deployment is finished, the log files are moved to `/tmp`\.

By default, your provisioned Amazon EC2 instances are retained when a deployment fails\. If you created your Launch Wizard deployment with these default settings, you can navigate to the following paths for further evaluation\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-troubleshooting.html)

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

## Support<a name="launch-wizard-sap-support"></a>

If your deployment is failing after following the troubleshooting steps listed here, we recommend you to create a support case with the following information\.

```
            [Error description]:<Provide a brief description of the error.>
            
            [Deployment information]: Provide information about the failed deployment.
            Account number: <AWS account number>
            Deployment name: <Enter deployment name>
            Deployment type: <Single-instance/Multi-instance/High availability>
            SAP HANA version: <Enter SAP HANA database version>
            SAP application: <Enter SAP application name>
            OS type: <Enter operating system>
            OS version: <Enter operating system version>
            Amazon EC2 instance family: <Enter Amazon EC2 instance family>
            Amazon EC2 instance type: <Enter Amazon EC2 instance type>
            If used proxy: <Yes/No>
            AMI type: <BYOI/BYOS/Marketplace>
            Instances retained: <Yes/No>
            FailedStackID (optional): 
            
            [Required logs] Provide the following logs. Based on the scenario and state of deployment, some logs may not be available.
            /root/install/scripts/log/
            /tmp/install.log
            /tmp/inputs.json
            /var/log/cloud-init.log
            /var/log/hdblcm.log (If SAP HANA install is selected)
            /tmp/NW directory (If SAP HANA install is selected)
            
            If you haven't retained your Amazon EC2 instance, provide the logs extracted from CloudWatch logs.
            
            [Troubleshooting]
            Provide the details of the troubleshooting steps that you carried out and the results from them.
```

For more information, see [Creating a support case](https://docs.aws.amazon.com/awssupport/latest/user/case-management.html#creating-a-support-case)\.