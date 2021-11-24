# Launch AWS Service Catalog products with Jira<a name="launch-wizard-sap-service-catalog-jira"></a>

AWS Service Catalog products created with AWS Launch Wizard can be integrated with Jira workflows\. You can use the AWS Service Catalog Connector for Jira to natively provision and operate AWS Service Catalog products created with Launch Wizard by using Atlassian's Jira Service Management\. This workflow simplifies product request actions for Jira Service Management users and provides Jira Service Management governance and oversight over AWS products\.

**To use Jira to launch products, you must follow these prerequisites:**
+ Create a deployment using Launch Wizard by choosing the **Create an AWS Service Catalog product** option in the infrastructure settings in Launch Wizard\. For more information, see [Define infrastructure](launch-wizard-sap-deploying.md#launch-wizard-sap-infrastructure)\.
+ Install the AWS Service Catalog Connector for Jira\. For information about how to install the Connector, see [AWS Service Management Connector for ServiceNow](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/integrations-jiraservicedesk.html)\.
+ Complete the [set up steps to launch AWS Service Catalog products](launch-wizard-sap-service-catalog.md#launch-wizard-sap-service-catalog-setup)\.
+ Complete the steps to [create a launch constraint](launch-wizard-sap-service-catalog.md#launch-wizard-sap-service-catalog-constraint)\. 
+ Make public the location within the S3 bucket that you provided in your Launch Wizard infrastructure settings, by doing the following:

  1. Navigate to the [Amazon S3 console](https://console.aws.amazon.com/s3)\.

  1. Locate the name of the Amazon S3 location that you specified when you [defined the infrastructure for your Launch Wizard deployment](launch-wizard-sap-deploying.md#launch-wizard-sap-infrastructure)\. 

  1. Under the folder that you specified, locate a new folder named `<LaunchWizardDeploymentName>-<TimeStamp>`\. This is the folder to which the Launch Wizard service copies the AWS CloudFormation templates and deployment artifacts\. Select this folder and make it public\.

For more information about how to integrate AWS products into your Jira Service Management portal using the AWS Service Catalog Connector, watch the following video\.

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/1AODGjhqufo/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/1AODGjhqufo)