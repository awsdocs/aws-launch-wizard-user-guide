# Deploy SAP applications with AWS Launch Wizard for SAP using a proxy server<a name="launch-wizard-sap-deploy-proxy-server"></a>

AWS Launch Wizard for SAP launches and configures Amazon EC2 instances to deploy an SAP system on AWS\. The launched instances must have outbound connectivity to internet to download operating system patches and communicate with several AWS services\. You can setup this connection via an internet gateway or a proxy server in a public subnet\.

The following is an example on how to configure a Squid proxy server for deploying SAP applications on AWS with Launch Wizard\.

**Topics**
+ [Setup](#setup-proxy-server)
+ [Run Launch Wizard](#run-proxy-server)
+ [Troubleshoot](#troubleshoot-proxy-server)

## Setup<a name="setup-proxy-server"></a>

Configure your Squid proxy server with the following steps\.

1. Choose any Linux\-based AMI\. In this example, we have selected SLES 12 SP5 for SAP AMI\.

1. Verify that your server is hosted on a public subnet and is attached to a public IP address\.

1. Add AWS services to the `allowed_list` file\. 

   1. In the Squid server configuration file `/etc/squid/squid.conf`, create an `allowed_list` path using the `acl` command\.

      ```
      acl whitelist dstdomain '/etc/squid/allowed_list'
      ```

   1. In the `allowed_list` file, add the domains of all the services listed in the following table\.

   1. Run the `rcsquid restart` command for the changes to take effect\.


| Service name | Domains to be allowed | 
| --- | --- | 
| Amazon DynamoDB |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| Amazon EFS |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| Amazon EBS |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| Amazon EC2 |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| AWS Lambda |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| Amazon Route 53 |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| Amazon CloudWatch |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| AWS CloudFormation |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| AWS KMS |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| AWS Secrets Manager |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| AWS Identity and Access Management |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| AWS Systems Manager |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| Amazon S3 |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| AWS CLI |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| SUSE infrastructure for SLES |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| SUSE packages | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html) | 
| REDHAT repository |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| Python packages |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| Amazon Cognito |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 
| Amazon Security Token Service |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploy-proxy-server.html)  | 

## Run Launch Wizard<a name="run-proxy-server"></a>

 After you complete the initial setup, you can begin deploying your SAP application using Launch Wizard\. For more information, see [Deploy an SAP application with AWS Launch Wizard](https://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sap-deploying.html)\. 

To connect your SAP deployment on Launch Wizard with the Squid proxy server, enter the IP address of the server\. To add the server address, go to Step 2 Define infrastructure > Infrastructure \- SAP landscape > Security groups >** Proxy server address \- optional\.**

The *No proxy* setting contains the list of whitelisted domains and IP addresses that do not pass through the proxy server\. 

 In the *No proxy setting \- optional* field, you must include the following IP addresses: 
+ Localhost \- `127.0.0.1`
+ Internal
+ Amazon EC2 instance metadata\- `169.254.169.254`

**Note**  
Include the hostnames of ASCS, ERS, primary SAP HANA, and secondary SAP HANA instances in the *No proxy setting \- optional* field, if you are deploying an SAP system with high availability using RHEL operating system\. This will enable the cluster to communicate with all the nodes as well as perform any failover or failback operations\.

### Amazon EC2 connection<a name="ec2-connect-proxy-server"></a>

Your Amazon EC2 instance must be connected to the SUSE repository servers on AWS\. Add the following IP addresses to the route tables of the associated Amazon EC2 instances\. For more information, see [Add and remove routes from a route table](https://docs.aws.amazon.com/vpc/latest/userguide/WorkWithRouteTables.html#AddRemoveRoutes)\. The *Target* of these routes should be the NAT gateway of your subnet\. For more information, see [Add a NAT Gateway to an Existing VPC](https://docs.aws.amazon.com/appstream2/latest/developerguide/add-nat-gateway-existing-vpc.html)\.
+ `34.197.223.242/32`
+ `54.197.240.216/32`
+ `54.225.105.144/32`
+ `107.22.231.220/32`

## Troubleshoot<a name="troubleshoot-proxy-server"></a>

To resolve any connectivity issues with the Squid proxy server, use the following steps\.

1. Login to your Squid proxy server\.

1. Open the `access.log` file located at `/var/log/squid/access.log`\.

1. Search for the **TCP\_DENIED** message in the `access.log` file\. The message displays an address that is not allowed in the proxy configuration\.

1.  Add the address to the `squid.conf` file and restart the Squid server for the changes to take effect\. 

1. You can now start over your SAP deployment with Launch Wizard\.

**Note**  
The troubleshooting steps are only applicable to the Squid proxy server\. The location of the `log` file varies with the type of proxy server\.