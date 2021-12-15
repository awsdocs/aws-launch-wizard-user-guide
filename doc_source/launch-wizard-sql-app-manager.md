# Manage Launch Wizard application resources with AWS Systems Manager Application Manager<a name="launch-wizard-sql-app-manager"></a>

AWS Systems Manager Application Manager, a capability of AWS Systems Manager, helps you to investigate and remediate issues with your AWS resources that make up an application\. Application Manager aggregates operations information from multiple AWS services and Systems Manager capabilities to a single console\.

Application Manager automatically imports application resources created by Launch Wizard\. From the Application Manager console, you can view operations details and perform operations tasks\. You can also use runbooks, or SSM Automation documents, provided by Launch Wizard from the Application Manager console to manage or remediate issues with application components or resources\. 

For general information about AWS Systems Manager Application Manager, see [AWS SSM Application Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/application-manager.html) in the AWS Systems Manager User Guide\.

The following information is specific to the management of Launch Wizard application resources from the Application Manager console\.

**Topics**
+ [Use runbooks](#launch-wizard-sql-app-manager-runbooks)
+ [Onboard existing applications](#launch-wizard-sql-app-manager-ops-metadata)
+ [Patch management](#launch-wizard-sql-app-manager-patch-manager)

## Use SSM Application Manager to run Automation workflows on your Launch Wizard applications<a name="launch-wizard-sql-app-manager-runbooks"></a>

You can perform operations tasks and remediate issues with your Launch Wizard application resources by using AWS Systems Manager Automation runbooks\.

Application Manager automatically imports all of your Launch Wizard resources and lists them in the Launch Wizard category\. From the Application Manager console, choose **Launch Wizard** from the list of **Applications**\. Select an application to view its information\. On the **Application information** page, choose **Start runbook**\. A dropdown list displays all of the runbooks available for your Launch Wizard application\. This list includes runbooks provided by AWS, as well as any custom runbooks you own or are shared with you\.

When you select a runbook, you are taken to the SSM Automation document console, where the resource group that makes up your application is preselected\.

For descriptions of the runbooks provided by **Launch Wizard**, see [AWS Launch Wizard Systems Manager Automation documents](launch-wizard-sql-provided-runbooks.md)\.

**Add custom runbooks**

To add your own runbooks, you must modify the service setting value for the supported type\.

1. The service setting value is a list of document Amazon Resource Names \(ARNs\)\. You can view this list using the following AWS Command Line Interface \(AWS CLI\) command, and adding the type to the `setting id` path\. 

   There are four supported types for which there are service settings:
   + `AWS-SQLServerWindows`
   + `AWS-SQLServerLinux`
   + `AWS-SAP`
   + `AWS-SelfManagedActiveDirectory`

   The following command lists the service settings for `AWS-SQLServerWindows`\.

   ```
   aws ssm get-service-setting --setting-id /launchwizard/AWS-SQLServerWindows
   ```

   The following is the example output\.

   ```
   {
       "ServiceSetting": {
           "SettingId": "/launchwizard/AWS-SQLServerWindows",
           "SettingValue": "arn:aws:ssm:us-east-1::document/AWSSQLServer-Backup,arn:aws:ssm:us-east-1::document/AWSSQLServer-Restore,arn:aws:ssm:us-east-1::document/AWSSQLServer-Index,arn:aws:ssm:us-east-1::document/AWSSQLServer-DBCC",
           "LastModifiedDate": "2020-11-13T13:36:09.527000-05:00",
           "LastModifiedUser": "System",
           "ARN": "arn:aws:ssm:us-east-1:012345678901:servicesetting/launchwizard/AWS-SQLServerWindows",
           "Status": "Default"
       }
   }
   ```

1. You can modify the list of document ARNs by running the following command\.

   ```
   aws ssm update-service-setting \
       --setting-id /launchwizard/AWS-SQLServerWindows \
       --setting-value \
       "arn:aws:ssm:us-east-1::document/AWSSQLServer-Backup,arn:aws:ssm:us-east-1::document/AWSSQLServer-Restore,arn:aws:ssm:us-east-1::document/AWSSQLServer-Index,arn:aws:ssm:us-east-1::document/Document"
   ```

1. To reset the service setting value, run the following AWS CLI command\. This command resets the service setting value for `AWS-SQLServerWindows`\.

   ```
   aws ssm reset-service-setting --setting-id /launchwizard/AWS-SQLServerWindows
   ```

   The following is the example output\.

   ```
   {
       "ServiceSetting": {
           "SettingId": "/launchwizard/AWS-SQLServerWindows",
           "SettingValue": "arn:aws:ssm:us-east-1::document/AWSSQLServer-Backup,arn:aws:ssm:us-east-1::document/AWSSQLServer-Restore,arn:aws:ssm:us-east-1::document/AWSSQLServer-Index,arn:aws:ssm:us-east-1::document/AWSSQLServer-DBCC",
           "LastModifiedDate": "2020-11-13T13:36:09.527000-05:00",
           "LastModifiedUser": "System",
           "ARN": "arn:aws:ssm:us-east-1:012345678901:servicesetting/launchwizard/AWS-SQLServerWindows",
           "Status": "Default"
       }
   }
   ```

   The document lists correspond to the application type level\. Therefore, when you add a new `AWS-SQLServerWindows` document, it will show up in all `AWS-SQLServerWindows` deployments\. You can't add documents to a specific application\.
**Note**  
Verify that you use the correct Region for the added document ARNs\. 

## Onboard existing applications<a name="launch-wizard-sql-app-manager-ops-metadata"></a>

When you deploy an application with Launch Wizard, the resource groups that make up the application are automatically assigned metadata showing that they are provisioned by Launch Wizard\. Application Manager uses this metadata to display all of your resource groups and AWS CloudFormation stacks created by Launch Wizard on one page\. When you deploy an application, Launch Wizard calls the `CreateOpsMetadata` API to assign the provisioning metadata\. 

**Onboard existing applications**  
You can manually call the `CreateOpsMetadata` API using the AWS CLI so that existing application deployments appear on the Application Manager Launch Wizard page\. The following example shows the `create-ops-metadata` AWS CLI command\.

```
aws ssm create-ops-metadata \
    --resource-id "arn:aws:resource-groups:us-east-1:123456789012:group/LaunchWizard-SQLHAAlwaysOn-test" \ 
    --metadata '{"application-type": {"Value": "AWS-SQLServerWindows"}, "provisioned-by": {"Value": "AWS-LaunchWizard"}}'
```

You must provide the following information:
+ The resource group ARN of the resource that you want to be visible on the Launch Wizard page in Application Manager\.
+ A metadata JSON file that contains the `application-type` and `provisioned-by` key values\. The `application-type` is the application type of the deployment, for example `AWS-SQLServerWindows` or `AWS-SAP`\. The `provisioned-by` value is `AWS-LaunchWizard`\.

When the command is successful, the output will be an `OpsMetadataArn`\. If the output is an `OpsMetadataAlreadyExistsException`, then the resource group has already been tagged\.

**View all `OpsMetadata` values**  
You can call the `ListOpsMetadata` API to view all of your `OpsMetadata` values\. To display only Launch Wizard\-related metadata objects, you can use filtering\. The following example shows the `list-ops-metadata` AWS CLI command\.

```
aws ssm list-ops-metadata \
    --filters '[{"Key":"provisioned-by","Values":["AWS-LaunchWizard"]}]' \
    --max-results 20
```

The following is the example output\.

```
{
    "OpsMetadataList": [
        {
            "ResourceId": "arn:aws:resource-groups:us-east-1:123456789012:group/LaunchWizard-SQLHAAlwaysOn-test",
            "OpsMetadataArn": "arn:aws:ssm:us-east-1:123456789012:opsmetadata/aws/ssm/LaunchWizard-SQLHAAlwaysOn-test/appmanager",
            "LastModifiedDate": "2020-11-16T22:41:43.035000-05:00",
            "LastModifiedUser": "arn:aws:sts::123456789012:assumed-role/Admin",
            "CreationDate": "2020-11-16T22:41:43.035000-05:00"
        }
    ]
}
```

**Filter by application type**  
The following example shows the `list-ops-metadata` AWS CLI command to filter by application type:

```
aws ssm list-ops-metadata \
    --filters '[{"Key":"application-type","Values":["AWS-SQLServerWindows","AWS-SAP"]}]' \
    --max-results 20
```

To get information about an `OpsMetadataArn` object, use the following command and enter the `OpsMetadataArn`\.

```
aws ssm get-ops-metadata \ 
    --ops-metadata-arn "arn:aws:ssm:us-east-1:123456789012:opsmetadata/aws/ssm/LaunchWizard-SQLHAAlwaysOn-test/appmanager"
```

The following is the example output\.

```
{
    "ResourceId": "arn:aws:resource-groups:us-east-1:123456789012:group/LaunchWizard-SQLHAAlwaysOn-test",
    "Metadata": {
        "application-type": {
            "Value": "AWS-SQLServerWindows"
        },
        "provisioned-by": {
            "Value": "AWS-LaunchWizard"
        }
    }
}
```

**Delete metadata object**  
You can delete the metadata object if you make a mistake when using the `create-ops-metadata` AWS CLI command\. Run the following command, entering the `OpsMetadataArn`, and then run the `create-ops-metadata` command again\.

```
aws ssm delete-ops-metadata \
    --ops-metadata-arn "arn:aws:ssm:us-east-1:123456789012:opsmetadata/aws/ssm/LaunchWizard-SQLHAAlwaysOn-test/appmanager"
```

For more information about `CreateOpsMetadata` and related APIs, see the [Amazon EC2 Systems Manager API Reference](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateOpsMetadata.html)\.

## Patch management<a name="launch-wizard-sql-app-manager-patch-manager"></a>

You can automate the process of patching your Launch Wizard instances with security and other types of updates\. From the **Application information** page of the Application Manager console, choose **Patch**\. You are taken to the SSM Patch Manager console **Patch now** page, where patch management options for your application instances are preselected\.

For more information about how Patch Manager determines which patches to install and how it installs them, see [How Patch Manager operations work](https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-how-it-works.html)\.