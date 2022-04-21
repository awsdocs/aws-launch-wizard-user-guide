# Post\-deployment steps<a name="launch-wizard-exchange-post-deployment"></a>

Post\-deployment steps for Exchange Server on AWS

**Topics**
+ [\(Optional\) Run Windows Updates](#launch-wizard-iis-updates)
+ [Create database copies](#exchange-create-db-copies)
+ [\(Optional\) Creating a DNS entry for the load balancer](#exchange-elb-dns)

## \(Optional\) Run Windows Updates<a name="launch-wizard-iis-updates"></a>

To help ensure that the deployed servers' operating systems and installed applications have the latest Microsoft updates, run Windows Update on each server\.

### Install Windows Updates on your RD Gateways using public IP addresses<a name="launch-wizard-iis-updates-bastion.title"></a>

To install Windows updates on the RD Gateways with their public IP addresses:

1. Identify the public IP addresses for the RD Gateways, from the [Amazon EC2 Console](https://console.aws.amazon.com/ec2/)\.

1. Use the public IP of the RD Gateway to [connect to the instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html)\.

1. On the taskbar, open the **Start** menu, and choose **Settings**\.

1. In the Settings application, choose **Update & Security**

1. Choose **Check for updates**\.

1. Install any updates, and restart if necessary\.

### Install Windows Updates on your Exchange Servers by connecting through an RD Gateway or bastion host<a name="launch-wizard-iis-updates-bastion.title"></a>

To install Windows updates on the Exchange servers by connecting from within a public resource such as an RD Gateway or bastion host:

1. Identify the public IP addresses for the public resource, and also the private IP addresses of the Exchange servers, from the [Amazon EC2 Console](https://console.aws.amazon.com/ec2/)\.

1. Use the public IP of the public resource to [connect to the instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html)\.

1. From within the RDP connection to the public resource, use the Exchange server's private IP addresses when creating subsequent RDP connections\.
**Note**  
You will use the nested RDP session within the public resource to the Exchange servers for the remaining steps\.

1. On the taskbar, open the **Start** menu, and choose **Settings**\.

1. In the Settings application, choose **Update & Security**

1. Choose **Check for updates**\.

1. Install any updates, and restart if necessary\.

## Create database copies<a name="exchange-create-db-copies"></a>

Launch Wizard for Exchange Server creates a [database availability groups](https://docs.microsoft.com/en-us/exchange/database-availability-groups-dags-exchange-2013-help?redirectedfrom=MSDN) \(DAG\) and adds the Exchange nodes to the DAG\. As part of the Exchange Server installation, each Exchange node contains a mailbox database\. The first node contains a database called *DB1*, and the second node contains a database called *DB2*\.

As part of configuring high availability for the mailbox roles, you can add mailbox database copies on the other Exchange nodes\. Alternatively, you can create entirely new databases and only then create additional copies\.

```
Add-MailboxDatabaseCopy -Identity DB1 –MailboxServer ExchangeNode2 -ActivationPreference 2
Add-MailboxDatabaseCopy -Identity DB2 –MailboxServer ExchangeNode1 -ActivationPreference 2
```

## \(Optional\) Creating a DNS entry for the load balancer<a name="exchange-elb-dns"></a>

If you chose to deploy a load balancer, these steps guide you through creating a DNS entry so that traffic can be distributed to your Exchange nodes\.

**To create a DNS entry for a load balancer**

1.  If you chose to deploy a load balancer, it will have an endpoint address such as *elb\.amazonaws\.com*\. 

1. To use the load balancer with your Exchange namespace, create a CNAME record in Active Directory that points to the load balancer\.

1. Before proceeding, go to the [Amazon EC2 Console](https://console.aws.amazon.com/ec2) and, under **Load balancer**, select the load balancer that Launch Wizard created\.

1. Copy the value listed under the DNS name as shown in the following image:  
![\[An image of the load balancer DNS name.\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/exchange-elb-dns-1.png)

1.  To create the DNS record, connect using Remote Desktop to one of the domain controllers using domain credentials, and open the DNS console by going to the **Start** menu and typing *DNS*\.

1. In the DNS console, navigate to the Active Directory zone, open the context \(right\-click\) menu on the zone, and select **New Alias \(CNAME\)**, as shown in the following image:  
![\[An image of creating a CNAME record - part 1.\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/exchange-elb-dns-2.png)

1. For **Alias Name**, specify an entry such as *mail*, and for **fully qualified domain name \(FQDN\) for target host**, paste the value of the load balancer endpoint\. The following image shows example entries:  
![\[An image of creating a CNAME record - part 2.\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/exchange-elb-dns-3.png)

1. Verify that the DNS entry is resolved successfully by using a computer that should be able to resolve the entry with your Active Directory domain name\. On the taskbar of such a resource, open the **Start** menu, and type **cmd**\. In the command line window, use the name of the CNAME record you created in place of *mail*, and your Active Directory domain name in place of *example\.com*:

   ```
   nslookup mail.example.com
   ```

1. Check that the record resolves to the load balancer DNS record, such as in the following image:  
![\[An image of verifying the CNAME DNS record resolution.\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/exchange-elb-dns-4.png)