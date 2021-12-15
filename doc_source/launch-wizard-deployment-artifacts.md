# Repeat SAP application deployments using deployment artifacts created with AWS Launch Wizard<a name="launch-wizard-deployment-artifacts"></a>

This section contains information about how to repeat deployments using deployment artifacts created with Launch Wizard\. The artifacts include AWS Service Catalog products and AWS CloudFormation templates\.

**Topics**
+ [How AWS Launch Wizard integration with AWS Service Catalog works](#launch-wizard-sap-service-catalog-how-it-works)
+ [Launch AWS Service Catalog products created with AWS Launch Wizard](launch-wizard-sap-service-catalog.md)
+ [Launch AWS Service Catalog products with ServiceNow](launch-wizard-sap-service-catalog-servicenow.md)
+ [Launch AWS Service Catalog products with Jira](launch-wizard-sap-service-catalog-jira.md)
+ [Launch AWS Service Catalog products with Terraform](launch-wizard-sap-service-catalog-terraform.md)
+ [Launch AWS CloudFormation templates created in Launch Wizard](launch-wizard-sap-launch-artifacts-cloudformation.md)

## How AWS Launch Wizard integration with AWS Service Catalog works<a name="launch-wizard-sap-service-catalog-how-it-works"></a>

AWS Launch Wizard creates AWS Service Catalog products from successful deployments\. The AWS Service Catalog products contain AWS CloudFormation templates and associated application configuration scripts, which are stored in Amazon S3\. You can use the AWS Service Catalog products, along with integrations offered by AWS Service Catalog, with third\-party products, such as ServiceNow, Jira, or Terraform\. Or, you can use the AWS CloudFormation templates and application configuration scripts saved in Amazon S3 to deploy SAP applications that meet the requirements of organizational deployment and governance policies\.

In addition to supporting deployments using AWS CloudFormation templates, AWS Service Catalog, and multiple deployment tools supported by AWS Service Catalog, AWS Launch Wizard creates a point\-in\-time snapshot of the code used to deploy and configure SAP applications at the time of the deployment\. You can use the code "as is" for consistent repeated deployments, or you can use the code as a baseline and update it to meet specific application requirements\.

AWS Launch Wizard creates a default Launch Wizard portfolio and products within the portfolio\. An AWS Service Catalog product is created for each deployment and given a name that corresponds to the Launch Wizard deployment name\.

![\[Deploying SAP applications with Launch Wizard, AWS CloudFormation, AWS Service Catalog, and third-party applications\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/lw-sc-architecture.png)