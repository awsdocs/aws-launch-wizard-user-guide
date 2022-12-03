# Troubleshoot AWS Launch Wizard for SQL Server<a name="launch-wizard-troubleshooting"></a>

Each application in your account in the same AWS Region can be uniquely identified by the application name specified at the time of a deployment\. The application name can be used to view the details related to the application launch\.

For SQL Server deployments on Linux, you must use an instance type built on the [Nitro System](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html#ec2-nitro-instances)\. EBS volumes are exposed as NVMe block devices on instances built with the Nitro System\. Device names that are specified for NVMe EBS volumes in a block device mapping are renamed using NVMe device names \(`/dev/nvme[[0-26]n1`\)\. Launch Wizard deployments on Linux do not support block devices on Xen\-virtualized instances\. 

**Topics**
+ [Active Directory objects and DNS record clean up \(deployment on Windows\)](#launch-wizard-ad-dns-clean)
+ [Launch Wizard provisioning events](#launch-wizard-provisioning)
+ [CloudWatch Logs](#launch-wizard-logs)
+ [AWS CloudFormation stack](#launch-wizard-cloudformation)
+ [Pacemaker on Ubuntu \(deployment on Linux\)](#launch-wizard-pacemaker)
+ [SQL Server Management Studio](#launch-wizard-troubleshooting-ssms)
+ [Errors](#launch-wizard-errors)

## Active Directory objects and DNS record clean up \(deployment on Windows\)<a name="launch-wizard-ad-dns-clean"></a>

When you delete a deployment, you lose all specification settings for the SQL Server Always On application\. Launch Wizard attempts to delete only the AWS resources that it created in your account as part of the deployment\. If you created resources outside of Launch Wizard, for example, resources in a VPC created by Launch Wizard, the deletion can fail\. Launch Wizard does not delete Active Directory objects in your Active Directory, nor does it delete any of the records in your DNS server\. Launch Wizard has no control over your Active Directory domain user password over time, which is required to clean up Active Directory objects or DNS records\. We recommend that you remove these entries from your Active Directory after Launch Wizard deletes the deployment\.

If the initial Active Directory objects or DNS records are not cleaned up, when you attempt to deploy Launch Wizard on an existing Active Directory using a deployment name that has already been used or availability group/listener/cluster name that has already been used, the deployment may fail with the following error\.

**Error message**

`System.Management.Automation.Remoting.PSRemotingTransportException: Connecting to remote server xxxxxx failed with the following error message : WinRM cannot complete the operation. Verify that the specified computer name is valid, that the computer is accessible over the network, and that a firewall exception for the WinRM service is enabled and allows access from this computer. By default, the WinRM firewall exception for public profiles limits access to remote computers within the same local subnet.`

To address this error, we recommend that you remove the initial entries from your Active Directory\.

To clean up Active Directory Objects, run the following example PowerShell commands as a domain user with the appropriate authorization to perform these operations\.

`$Pwd = ConvertTo-SecureString $password -AsPlainText –Force`

`$cred = New-Object System.Management.Automation.PSCredential $domainUser, $Pwd`

`$ADObject = Get-ADObject -Filter 'DNSHostName -eq “SQLnode.example.com”`

`Remove-ADObject -Recursive -Identity $ADObject -Credential $cred`

To remove a DNS Record, the name of the record that you want to delete \(SQL Server node name\), the DNS server FQDN, and the DNS zone within which the record is residing are required\. The following are example PowerShell commands to perform the DNS record removal\.

`$NodeDNS = Get-DnsServerResourceRecord -ZoneName $ZoneName -ComputerName $DNSServer -Node $NodeToDelete -RRType A -ErrorAction SilentlyContinue`

`Remove-DnsServerResourceRecord -ZoneName $ZoneName -ComputerName $DNSServer -InputObject $NodeDNS -Force`

## Launch Wizard provisioning events<a name="launch-wizard-provisioning"></a>

Launch Wizard captures events from SSM Automation and AWS CloudFormation to track the status of an ongoing application deployment\. If an application deployment fails, you can view the deployment events for this application by selecting **Deployments** from the navigation pane\. A failed event shows a status of **Failed** along with a failure message\. 

## CloudWatch Logs<a name="launch-wizard-logs"></a>

Launch Wizard streams provisioning logs from all of the AWS log sources, such as AWS CloudFormation, SSM, and CloudWatch Logs\. CloudWatch Logs for a given application name can be viewed on the CloudWatch console for the log group name `LaunchWizard-APPLICATION_NAME` and log stream `ApplicationLaunchLog`\. 

## AWS CloudFormation stack<a name="launch-wizard-cloudformation"></a>

Launch Wizard uses AWS CloudFormation to provision the infrastructure resources of an application\. CloudFormation stacks can be found in your account using the CloudFormation `describe-stacks` API\. Launch Wizard launches various stacks in your account for validation and application resource creation\. The following are the relevant filters for the `describe-stacks` API\.
+ **Validation**

  `LaunchWizard-APPLICATION_NAME-checkCredentials-SSM_execution_id`
+ **Validation**

  `LaunchWizard-APPLICATION_NAME-checkVPCConnectivity-SSM_execution_id`
+ **Application resources**

  `LaunchWizard-APPLICATION_NAME`\. This stack also has nested stacks for VPC, AD, the RDGW node, and SQL nodes\.

You can view the status of these CloudFormation stacks\. If any of them fail, you can view the cause of failure\.

## Pacemaker on Ubuntu \(deployment on Linux\)<a name="launch-wizard-pacemaker"></a>

To troubleshoot Pacemaker cluster resource issues, take the following actions as an administrator\.
+ Inspect the system log files for operating system errors and address the errors, as needed\.
+ Inspect the cluster log files for errors, including for errors that relate to Pacemaker, Corosync, or SQL Server\. Check the log files carefully because the related services may provide only one or two related log entries\. 
+ Verify resource configuration, and configuration of cluster\-related functions\.
  + The following commands display the configuration details:
    + To display all resources, use: `pcs resource show -full`\.
    + Or, you can use: `pcs resource show <resource name>`\.
  + The following command will display the cluster constraints: `pcs constraints –full`\.
  + The following command displays the cluster properties: `pcs property list –all`\.
+ Manually start the resource with `debug-start`\.
+ Clear failed actions with the following command: `pcs resource cleanup <resource name>`\.

## SQL Server Management Studio<a name="launch-wizard-troubleshooting-ssms"></a>

If you encounter issues when you attempt to add databases with SQL Server Management Studio, perform the following to add databases to the availability group:

1. Log in to the primary node using SQL Server Management Studio \(SSMS\) and record the name of the availability group\.

1. Verify that the database that you want to add to the availability group is backed up\.

1. Add the database to the availability group by running the following command in SSMS:

   ```
   ALTER AVAILABILITY GROUP ag-name ADD DATABASE db
   ```

1. Refresh the availability group and verify that the database was created\.

## Errors<a name="launch-wizard-errors"></a>

**Directory fails to create**
+ **Cause:** An internal service error has been encountered during the creation of the directory\.
+ **Solution:** Retry the operation\. For this scenario, you must retry the deployment from the initial page of the Launch Wizard console\.

**Your requested instance type is not supported in your requested Availability Zone**
+ **Cause:** This failure might happen during the launch of either your RDGW instance or your SQL Server instance, or during the validation of the instances that Launch Wizard launches in your selected subnets\. 
+ **Solution:** For this scenario, you must choose a different Availability Zone and retry the deployment from the initial page of the Launch Wizard console\.

**Validate connectivity for subnet\. The following resource\(s\) failed to create: \[ValidationNodeWaitCondition\]**

This failure can occur for multiple reasons\. The following list shows known causes and solutions\.
+ 

**VPC or subnet configuration does not meet prerequisites**
  + **Cause:** This failure occurs when your VPC or subnet configuration does not meet the prerequisites documented in the VPC Connectivity Section under [Deploy an application with AWS Launch Wizard for SQL Server on Windows](launch-wizard-deploying.md)\. If the failure message points to your selected public subnet, then the public subnet is not configured for outbound internet access\. If the failure message points to one of your selected private subnets, then the specified private subnet does not have outbound connectivity\. 
  + **Solution:** Check that your VPC includes one public subnet and, at least, two private subnets\. Your VPC must be associated with a DHCP Options Set to enable DNS translations to work\. The private subnets must have outbound connectivity to the internet and other AWS services \(S3, CFN, SSM, and Logs\)\. We recommend that you enable this connectivity with a NAT Gateway\. Note that, in the console, when you select a private subnet for the public subnet dropdown or you select a public subnet for the private subnet dropdown, you will encounter the same error\. Please refer to the VPC Connectivity section under [Deploy an application with AWS Launch Wizard for SQL Server on Windows](launch-wizard-deploying.md) for more information about how to configure your VPC\.
+ 

**EC2 instance stabilization error**
  + **Cause:** Failure can occur if the EC2 instance used for validation fails to stabilize\. When this happens, the EC2 instance is unable to communicate to the CloudFormation service to signal completions, resulting in `WaitCondition` errors\. 
  + **Solution:** Please contact [AWS Support](https://aws.amazon.com/support) for assistance\.