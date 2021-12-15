# AWS Launch Wizard Systems Manager Automation documents<a name="launch-wizard-sql-provided-runbooks"></a>

A Systems Manager Automation document defines the actions that Systems Manager performs on your managed instances and other AWS resources when an automation workflow runs\. A document contains one or more steps that run in sequential order\.

Launch Wizard provides predefined Automation documents that are maintained by AWS\. This topic describes each of the predefined Automation documents provided for AWS Launch Wizard\.

For more information about SSM Automation documents, see [AWS SSM Automation](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-automation.html) in the *AWS Systems Manager User Guide*\.

**Topics**
+ [AWSSQLServer\-DBCC](#launch-wizard-sql-runbooks-sqldbcc)
+ [AWSSQLServer\-Backup](#launch-wizard-sql-runbooks-sqlbackup)
+ [AWSSQLServer\-Index](#launch-wizard-sql-runbooks-sqlindex)
+ [AWSSQLServer\-Restore](#launch-wizard-sql-runbooks-sqlrestore)

## AWSSQLServer\-DBCC<a name="launch-wizard-sql-runbooks-sqldbcc"></a>

The `AWSSQLServer-DBCC` Automation document includes the steps to perform database integrity checks on a specified database\. You can control the type of database checks that are run\. You can also adjust the execution parameters, such as specific tables to check, maximum CPU utilization, and more\. For more information about the operations performed by DBCC checks, see the [SQL Server documentation](https://www.sqlservercentral.com/blogs/how-to-run-dbcc-checkdb-to-check-sql-database-integrity)\.

## AWSSQLServer\-Backup<a name="launch-wizard-sql-runbooks-sqlbackup"></a>

The `AWSSQLServer-Backup` Automation document includes the steps to back up a specified database in either full, differential, or transactional mode\. After the backup is completed, you can upload it to a specified folder within an S3 bucket\. 

The backup modes are defined as follows:
+ **Full** — a complete backup of the database\.
+ **Differential** — the delta of changes since the last full backup\.
+ **Transactional** — a log of changes from the last full or differential backup, depending on the last backup type taken\.

The following conditions must be met for the `AWSSQLServer-Backup` document to successfully back up a database:
+ Backups must be staged to the local disk to run `AWSSQLServer-Backup`\. 
+ The maximum size of the backup file for uploading to Amazon S3 is 500 GB or less\.

For more information about backup modes, see the [Microsoft documentation](https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/backup-overview-sql-server?view=sql-server-ver15)\.

## AWSSQLServer\-Index<a name="launch-wizard-sql-runbooks-sqlindex"></a>

The `AWSSQLServer-Index` Automation document includes steps to perform index maintenance operations on a specified database\. You can choose a configuration, which includes the specific actions to take based on the level of fragmentation\. 

For more information about index maintenance operations, see the [Microsoft documentation](https://docs.microsoft.com/en-us/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described?view=sql-server-ver15)\.

## AWSSQLServer\-Restore<a name="launch-wizard-sql-runbooks-sqlrestore"></a>

The `AWSSQLServer-Restore` Automation document includes steps to download a backup database from a specified S3 bucket and folder to local storage\. You can also optionally restore the backup to a copy of the database\. The default behavior is to use the latest backup, and you can specify a time range to perform a point\-in\-time restore\. The following conditions must be met for the `AWSSQLServer-Restore` document to successfully restore a database:
+ The backup to use must have been performed by the `AWSSQLServer-Backup` document\.
+ There must be at least one full backup that occurred during the specified time range\.