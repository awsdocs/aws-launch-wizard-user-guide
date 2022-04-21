# What Is AWS Launch Wizard for Internet Information Services?<a name="what-is-launch-wizard-iis"></a>

AWS Launch Wizard is a service that guides you through the sizing, configuration, and deployment of a Windows Server workload running Internet Information Services \(IIS\) resources on AWS, following the [AWS Well\-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)\. IIS for Windows Server is a Web server which enables various use cases such as hosting web content and web applications\. The deployment includes best practices for configuring a highly available, fault\-tolerant, and secure IIS environment\.

This Launch Wizard deployment provides a guided console experience that uses CloudFormation templates for deployment\. The templates are based on the [Internet Information Services on AWS Quick Start](https://aws-quickstart.github.io/quickstart-microsoft-iis/)\. Launch Wizard reduces the time it takes to deploy IIS based solutions to the cloud\. Launch Wizard provides an estimated cost of deployment, and you can modify your resources and instantly view the updated cost assessment\. When you approve, Launch Wizard provisions and configures the selected resources to create a fully\-functioning production\-ready IIS application\. It also creates custom AWS CloudFormation templates, which can be reused and customized for subsequent deployments\.

The deployment consists of [Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/concepts.html) instances in an [Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html) group\. The instances are deployed in separate subnets across multiple Availability Zones for high availability\. The infrastructure provides a foundation for running many Microsoft solutions, such as Microsoft SharePoint and Microsoft \.NET Framework\.

The automation in the solution is provided by [Amazon EC2 Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html), [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html), and Windows PowerShell [Desired State Configuration](https://docs.microsoft.com/en-us/powershell/dsc/overview?view=dsc-1.1) \(DSC\)\. Amazon EC2 instances are configured using [lifecycle hooks](https://docs.aws.amazon.com/autoscaling/ec2/userguide/lifecycle-hooks.html), [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html), and Amazon EC2 Systems Manager Automation\.

**Topics**
+ [Deployment options](#launch-wizard-iis-deployment-types)
+ [Components](#launch-wizard-iis-components)

## Deployment options<a name="launch-wizard-iis-deployment-types"></a>

This Launch Wizard application provides the following deployment options:
+ **Deploy IIS into a new VPC\.** This option builds a new AWS environment consisting of a VPC, subnets, NAT gateways, security groups, bastion hosts, and other infrastructure components, and then deploys IIS into this new VPC\.
+ **Deploy IIS into an existing VPC\.** This option provisions IIS in your existing AWS infrastructure\.

## Components<a name="launch-wizard-iis-components"></a>

An IIS environment deployed with Launch Wizard will include the following components:
+ A highly available architecture that spans two Availability Zones\. \*
+ An Amazon VPC configured with public and private subnets according to AWS best practices, to provide you with your own virtual network on AWS\. \*
+ In the public subnets:
  + Managed network address translation \(NAT\) gateways to allow internet access for resources in the private subnets\. \*
  + Elastic Load Balancing is provided by an Application Load Balancer to distribute traffic across Amazon EC2 instances \(when using *internet\-facing* as the **Elastic Load Balancing scheme**\)\.
  + \(Optional\) Remote Desktop Gateways \(RD Gateways\) in an Amazon EC2 Auto Scaling group\.
+ In the private subnets:
  + Amazon EC2 Auto Scaling group of EC2 instances into which IIS is deployed\.
  + Elastic Load Balancing is provided by an Application Load Balancer to distribute traffic across Amazon EC2 instances \(when using *internal* as the **Elastic Load Balancing scheme**\)\.
  + AWS Managed Microsoft AD\.
+ Amazon EventBridge, providing the rules that initiate automation routines in response to Amazon EC2 Auto Scaling events\.
+ Amazon EC2 Systems Manager to store automation documents\.
+ AWS Identity and Access Management \(IAM\) roles\.
+ Security groups to control traffic to your EC2 instances\.
+ S3 bucket for storing [Managed Object Format](https://docs.microsoft.com/en-us/windows/win32/wmisdk/managed-object-format--mof-) \(MOF\) files\.

\* When you deploy IIS into an existing VPC, the components marked by asterisks are not created\. You will be prompted to enter resource IDs from your existing VPC\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/./images/iis-on-aws-architecture_diagram.png)