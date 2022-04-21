# Scale SAP applications with AWS Launch Wizard for SAP after initial deployment<a name="launch-wizard-sap-add-remove-nodes"></a>

You can scale an SAP application horizontally to meet increased performance requirements, depending on the initial SAP product and deployment pattern\. This section describes how you can add additional nodes to a preexisting SAP application deployed with AWS Launch Wizard for SAP\. It includes the prerequisites required to use this feature, and manual activities that you must perform after you add nodes\.

**Topics**
+ [Shared responsibility](#launch-wizard-sap-add-remove-nodes-shared-responsiblity)
+ [Prerequisites](#launch-wizard-sap-add-remove-nodes-prerequisites)
+ [Create image](#launch-wizard-sap-add-remove-nodes-create-image)
+ [Supported scenarios](#launch-wizard-sap-add-remove-nodes-scenarios)
+ [Pre\- and post\-deployment configuration scripts](#launch-wizard-sap-add-remove-nodes-pre-post-scripts)

## Shared responsibility model<a name="launch-wizard-sap-add-remove-nodes-shared-responsiblity"></a>

According to the [AWS Shared Responsibility Model](http://aws.amazon.com/compliance/shared-responsibility-model), scaling an SAP application that was deployed with AWS Launch Wizard requires prescribed manual activities that you must complete before adding or removing nodes\. These manual activities may vary depending on the scenario, for example, adding an application or database node\. The activities may also vary based on the source deployment architecture, for example, multi\-node or single\-node\. For example, you assume the responsibility of creating an Amazon Machine Image \(AMI\) of an existing application server or HANA worker node, upon which this feature depends\. It is your responsibility to ensure that the provided image is bootable in the VPC and subnet, and that Launch Wizard can access the instance through AWS Systems Manager\. AWS relieves the operational burden by using the image to provision the infrastructure as a new instance, and by installing the application, configuringit, or both\.

## Prerequisites for creating an AMI<a name="launch-wizard-sap-add-remove-nodes-prerequisites"></a>

Before you create an AMI to attach additional nodes, make sure that you:
+ Have a successful initial deployment\.
+ Create an AMI from the deployment to which you are adding the node\. If you want to add nodes to multiple deployments, you must create an AMI \(application/HANA worker\) for each deployment\.

  When an application is provisioned with Launch Wizard, multiple packages are installed, and operating system parameters are adjusted to make the operating system compliant and ready to install SAP\. Periodically, these packages and parameters get updated and change over time\. In addition, there can be changes to the disk layout as the application grows\. This configuration drift is not tracked by Launch Wizard, and therefore Launch Wizard is not able to record and replay it\. To mitigate the challenges presented by configuration drift, you must create and provide the latest image \(AMI\) of an existing server, including all of its volumes, which Launch Wizard uses to create the additional server or HANA worker node\. This process allows Launch Wizard to create new nodes that are similar to the existing nodes in terms of operating system packages and versions, and storage\.

## Create an image for scaling SAP deployments<a name="launch-wizard-sap-add-remove-nodes-create-image"></a>

The following best practices for creating an image to add an additional application node to a previously deployed SAP application using Launch Wizard are general guidelines only\. They do not represent a complete solution\. These recommendations are offered as considerations that may not be appropriate or sufficient for your environment, depending on the activities that you performed on these instances after the Launch Wizard deployment\.

**General recommendations:**
+ Do not share images with untrusted accounts\.
+ Do not make public images that contain private or sensitive data\.
+ Apply all of the latest available operating system security patches\.

**Before you create an image:**
+ Temporarily adjust the SAP or HANA instance profile parameter `Autostart` to `0`\. Revert to the original value after the image is created\.
+ Temporarily disable any third\-party applications from starting on boot\.
+ Keep the file systems and volumes intact, and ensure that the image is bootable if the volumes are not attached\. If the volumes are not attached, the `/etc/fstab nofail` setting must be enabled\.
+ Perform any other temporary adjustments that you can make that won't impact the boot process\. Revert to the original values after the image is created\.

**Create an image**  
Run the following AWS Command Line Interface command to create an image using the no reboot option\. 

```
aws ec2 create-image --instance-id i-021345abcdef6789 --name "<My server>" --no-reboot
```

[EC2 Image Builder](http://aws.amazon.com/image-builder/) can help you build, test, deploy, and maintain images\. We strongly recommend that you test your images to validate the security posture\. [Amazon Inspector](http://aws.amazon.com/inspector) is one solution for validating the security and compliance posture of your images\.

## Supported scenarios for adding or removing nodes with Launch Wizard for SAP<a name="launch-wizard-sap-add-remove-nodes-scenarios"></a>

Add or remove an additional server or node by using an Amazon EC2 Systems Manager \(SSM\) document that is created in your account during runtime\. The following scenarios are supported for the source deployment\. 


| AWS Launch Wizard for SAP application type | Deployment architecture | Supported scenario | AMI required | 
| --- | --- | --- | --- | 
|  HANA  | Multi\-node | Add a HANA worker node | Worker node of source deployment | 
|  NetWeaver on HANA  |  Multi\-node  | Add an application server node | PAS/AAS of source deployment | 

**Topics**
+ [Add HANA worker node to existing HANA scale\-out installation](#launch-wizard-sap-add-remove-nodes-scenarios-hana-worker)
+ [Add additional application server \(AAS\) to existing NetWeaver distributed installation](#launch-wizard-sap-add-remove-nodes-scenarios-server)

### Add HANA worker node to existing HANA scale\-out installation<a name="launch-wizard-sap-add-remove-nodes-scenarios-hana-worker"></a>

**Prerequisites**  
For prerequisites, see [Prerequisites for creating an AMIPrerequisites](#launch-wizard-sap-add-remove-nodes-prerequisites)\.

**Assumptions**  
Before you proceed with this procedure, consider the following assumptions:
+ A worker node will be created using the same instance type as an existing worker node\. Verify that all of the worker nodes are running on the same Amazon EC2 instance type\.
+ All HANA nodes and respective services are up and running\.
+ There are no upgrades or patching in progress\.
+ No maintenance activities, such as backups, are in progress\.

**Workflow for adding a HANA worker node**  
When you add an additional HANA worker node to your existing HANA scale\-out installation, Launch Wizard for SAP performs the following:

1. An instance is created using the provided AMI\.

1. The hostname is updated when the instance boots\.

1. `/etc/hosts` is updated on the master node, and then the host file is synced to the newly created node\.

1. All abandoned services and processes are cleaned up\.

1. If pre\-deployment configuration scripts are provided, they are run\.

1. `/usr/sap`, `/hana/data`, and `/hana/log` folders are cleaned up\.

1. `saphostagent` is set up\.

1. The HANA worker node is set up using `add_hosts`\.

1. If post\-deployment configuration scripts are provided, they are run\.

**Console steps for adding a HANA worker node to an existing scale\-out installation**  
Perform the following steps in the Launch Wizard for SAP console to add a HANA worker node:

1. Navigate to the Launch Wizard for SAP **Deployments** page\.

1. Select the check box next to the deployment to which you want to add a new node\. From the **Action** menu, select **Deploy additional components**>**HANA worker node**\. You will be taken to the **Configure settings** page\.

1. Under **Settings for additional worker node**, specify the following parameters:
   + **HANA worker node AMI** — select the AMI to provision the worker node\. You must use the most recent version of the AMI generated from the source deployment to which the worker node is being added\.
   + **Hostname of worker node** — Enter the hostname of the Amazon EC2 instance on which the SAP system will be deployed\.
   + **Private IP address** \(optional\) — Select the private IP address to assign to the new instance\. If you do not provide an IP address, Launch Wizard assigns one for you\.

1. Under **Pre\-deployment configuration script**, optionally specify the following parameters:
   + **Deployment settings** — Select the check box to ignore all deployment failures and proceed with a deployment\. 
   + **Configuration script** — Add one or more configuration scripts, depending on the number of servers included in the deployment\. The scripts run in the order they are added\. You can view detailed execution logs or failure information in the Amazon CloudWatch logs after a deployment is complete\.

1. Under **Post\-deployment configuration script**, optionally specify the following parameters:
   + **Deployment settings** — Select the check box to ignore all deployment failures and proceed with a deployment\. 
   + **Configuration script** — Add one or more configuration scripts, depending on the number of servers included in the deployment\. The scripts run in the order they are added\. You can view detailed execution logs or failure information in the Amazon CloudWatch logs after a deployment is complete\.

1. Choose **Next** when the preceding parameters are specified, and the **Review infrastructure configuration** page opens\.

1. Under **Infrastructure configuration**, specify the following parameters:
   + **Key pair name** — Select a key pair to securely connect to your instance\.
   + **Virtual Private Cloud \(VPC\)** — the VPC in which the domain controllers will be deployed is the same as for the original deployment\.
   + **Private subnet** — The private subnet is determined by the subnet specified in your original deployment\.
   + **Security group assigned to database servers** — Select a security group that is currently assigned to a database node\.
   + **HANA password** — enter a password for the SAP HANA installation\.

1. Choose **Next** when you are satisfied with your infrastructure configuration selections\. You will be taken to the **Review** page\.

1. On the **Review** page, verify your settings and configuration\. Choose **Deploy** if you are satisfied with your selections\. To edit your selections, choose **Edit** or **Previous**\. It can take up to 10 minutes to create your new node\.

**Manual activities required**  
The following manual activities are required to successfully add a HANA worker node to an existing scale\-out installation\.
+ Host entries are updated only on the HANA master and newly added nodes\. Refresh `/etc/hosts` entries from the HANA master node on all of the other existing nodes\.
+ When the automation workflow runs, a new HANA worker node is attached to the existing HANA deployment\. The node is ready to be used\. A HANA table redistribution plan must be determined and performed\. For more information about how to redistribute the tables to the new nodes, see [Redistributing Tables in a Scaleout SAP HANA System](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.03/en-US/c6579b60d9761014ae59c8c868e6e054.html) in the SAP documentation\.
+ The newly added worker node is not set up in the same placement group\. Attach the new worker node to the placement group and restart all of the HANA nodes for the placement groups to take effect\. 

**Delete a HANA worker node from an existing scale\-out installation**  
The process of deleting a HANA worker node from an existing scale\-out installation is partially automated\. Before you delete a worker node, you must redistribute the data for a multi\-database container \(MDC\) before deleting the node\. For more information about how to redistribute the tables to the new nodes, see [Redistributing Tables in a Scaleout SAP HANA System](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.03/en-US/c6579b60d9761014ae59c8c868e6e054.html) in the SAP documentation\.

**Note**  
You can only delete a node that was created with the add or remove node feature using Launch Wizard for SAP\.

### Add additional application server \(AAS\) to existing NetWeaver distributed installation<a name="launch-wizard-sap-add-remove-nodes-scenarios-server"></a>

**Prerequisites**  
For prerequisites, see [Prerequisites for creating an AMIPrerequisites](#launch-wizard-sap-add-remove-nodes-prerequisites)\.

**Assumptions**  
The following assumptions should be considered before proceeding with this procedure\.
+ All servers of the SAP application to which the node is being added are running\.
+ There are no upgrades or patching in progress\.

**Workflow for adding an additional application server \(AAS\) node**  
When you add an additional SAP server to your existing NetWeaver distributed installation, depending on whether you provide a PAS or AAS image, the file systems and volumes are adjusted to the requirements of the additional application server\. Launch Wizard for SAP performs the following:

1. An instance is created using the provided AMI\.

1. The hostname is updated when the instance boots\.

1. All abandoned services and processes are cleaned up\.

1. `/etc/hosts` is updated on the master node, and then the hosts file is synced to the new node\.

1. If the provided AMI is from the Primary Application Server \(PAS\) and Launch Wizard detects the `/samnt` volume in `/etc/fstab/`, the volume is unmounted and removed\. `/sapmnt` is mounted as an NFS from the PAS\.

1. If pre\-deployment configuration scripts are provided, they are run\.

1. `.env` files are updated and a new instance profile file is created\.

1. Services are started\.

1. The instance is started\.

1. If post\-deployment configuration scripts are provided, they are run\.

**Console steps for adding an AAS node to an existing NetWeaver distributed installation**  
Perform the following steps in the Launch Wizard for SAP console to add an AAS node:

1. Navigate to the Launch Wizard for SAP **Deployments** page\.

1. Select the check box next to the deployment to which you want to add a new node\. From the **Action** menu, select **Deploy additional components**>**Additional application server \(AAS\)**\. The **Configure settings** page opens\.

1. Under **Settings for additional worker node**, specify the following parameters:
   + **Additional application server \(AAS\) AMI** — Select the AMI to provision the additional application server\. You must use the most recent version of the AMI created from the source deployment\.
   + **Hostname of additional application server** — Enter the hostname of the Amazon EC2 instance on which the SAP system will be deployed\.
   + **Instance number of AAS** — Enter the instance number of the additional application server\.
   + **Private IP address** — Select the private IP address to assign to the new instance\.

1. Under **Pre\-deployment configuration script**, optionally specify the following parameters:
   + **Deployment settings** — Select the check box to ignore all deployment failures and proceed with a deployment\. 
   + **Configuration script** — Add one or more configuration scripts, depending on the number of servers included in the deployment\. The scripts run in the order they are added\. You can view detailed execution logs or failure information in the Amazon CloudWatch logs after a deployment is complete\.

1. Under **Post\-deployment configuration script**, optionally specify the following parameters:
   + **Deployment settings** — Select the check box to ignore all deployment failures and proceed with a deployment\. 
   + **Configuration script** — Add one or more configuration scripts, depending on the number of servers included in the deployment\. The scripts run in the order they are added\. You can view detailed execution logs or failure information in the Amazon CloudWatch logs after a deployment is complete\.

1. Choose **Next** when the preceding parameters are specified\. You will be taken to the **Review infrastructure configuration** page\.

1. Under **Infrastructure configuration**, specify the following parameters:
   + **Key pair name** — Select a key pair to securely connect to your instance\.
   + **Virtual Private Cloud \(VPC\)** — The VPC in which the domain controllers will be deployed is the same as for the original deployment\.
   + **Private subnet** — The private subnet is determined by the subnet specified in your original deployment\.
   + **Security group assigned to additional application server \(AAS\)** — Select a security group that is currently assigned to a database node\.

1. Choose **Next** when you are satisfied with your infrastructure configuration selections\. You will be taken to the **Review** page\.

1. On the **Review** page, verify your settings and configuration\. Choose **Deploy** if you are satisfied with your selections\. To edit your selections, choose **Edit** or **Previous**\. It can take up to 10 minutes to create your new node\.

**Manual activities required**  
When the automation workflow runs, the new node is attached to the existing SAP application and is reflected in the SAP console \(SM51\)\. The following manual activities are required to successfully add an AAS node to an existing Netweaver distributed installation\. The following steps are presented as general guidelines and may not be complete for your scenario\.
+ Host entries are updated only on the PAS and newly added nodes\. Refresh `/etc/hosts` entries from the PAS on all of the other existing nodes\.
+ Upload and set system profiles using transaction `RZ10`\.
+ Configure the number of work processes\.
+ Adjust or create logon and RFC server groups using transactions `SMLG` and `RZ12`\.
+ Adjust or create operation modes using transaction `RZ04`\.

**Delete an addition application server \(AAS\) from an existing Netweaver distributed installation\.**  
Before you delete an AAS node, verify that no users are logged in and no jobs are running on the instance\. 

**Note**  
You can only delete a node that was created with the add or remove node feature using Launch Wizard for SAP\.

## Pre\- and post\-deployment configuration scripts<a name="launch-wizard-sap-add-remove-nodes-pre-post-scripts"></a>

As with base node deployments with Launch Wizard for SAP, you can run pre\- and post\- deployment configuration scripts when you add an additional node\. For more information about how Launch Wizard for SAP accesses and deploys these scripts, see [Custom deployment configuration scripts](launch-wizard-sap-implementation.md#launch-wizard-sap-how-it-works-scripts)\.

**Limits for running pre\- and post\- deployment configuration scripts**  
The following limits apply when running pre\- and post\- deployment configuration scripts for your new node\.
+ Only one action can be performed at a time\.
+ SSM documents that are created at runtime are not deleted\. Periodic cleanup of the documents is required\.
+ Any automation activity performed on the new node is extremely limited\. Activities that must be performed on the parent node are called out in the SSM documentation\.