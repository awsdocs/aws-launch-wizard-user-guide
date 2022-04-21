# What is AWS Launch Wizard for Exchange Server?<a name="what-is-launch-wizard-exchange"></a>

AWS Launch Wizard for Exchange Server guides you through the sizing, configuration, and deployment of Exchange Server 2016 and Exchange Server 2019 environments on the AWS Cloud\. Exchange Server is a messaging and collaboration solution that Microsoft developed, with support for mailboxes, calendars, and e\-archival\. The deployment includes best practices for configuring a highly available, fault\-tolerant, and secure Exchange environment\.

This Launch Wizard deployment provides a guided console experience that uses CloudFormation templates for deployment\. The templates are based on the [Microsoft Exchange on the AWS Cloud Quick Start deployment guide](https://aws-quickstart.github.io/quickstart-microsoft-exchange/)\. Launch Wizard reduces the time it takes to deploy Exchange Server to the cloud\. Launch Wizard provides an estimated cost of deployment, and you can modify your resources and instantly view the updated cost assessment\. When you approve, Launch Wizard provisions and configures the selected resources to create a fully\-functioning production\-ready Exchange Server deployment\. It also creates custom AWS CloudFormation templates, which can be reused and customized for subsequent deployments\.

**Topics**
+ [Deployment options](#launch-wizard-exchange-deployments)
+ [Software Licensing](#launch-wizard-exchange-licensing)
+ [Components](launch-wizard-exchange-components.md)
+ [Implementation details](launch-wizard-exchange-details.md)

## Deployment options<a name="launch-wizard-exchange-deployments"></a>

Launch Wizard for exchange supports the following deployment type:
+ Deploy an Exchange environment into a new Amazon Virtual Private Cloud in your AWS account\.

## Software Licensing<a name="launch-wizard-exchange-licensing"></a>

Launch Wizard uses an evaluation copy of Exchange Server\. Exchange Server can be deployed and licensed through the [Microsoft License Mobility through Software Assurance](http://aws.amazon.com/windows/mslicensemobility/) program\. For development and test environments, you can use your existing MSDN licenses for Exchange Server using Amazon Elastic Compute Cloud \(Amazon EC2\) [Dedicated Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/dedicated-instance.html)\. For details, see the [MSDN on AWS](http://aws.amazon.com/windows/msdn/) page and [Exchange licensing FAQs](https://www.microsoft.com/en-us/microsoft-365/exchange/microsoft-exchange-licensing-faq-email-for-business) in the Microsoft documentation\.

Launch Wizard deploys the latest Amazon Machine Image \(AMI\) for Microsoft Windows Server 2016 and Windows Server 2019, and includes the license for the Windows Server operating system\. The AMI is updated on a regular basis with the latest service pack for the operating system\. The Windows Server AMI doesn’t require Client Access Licenses \(CALs\) and includes two Microsoft Remote Desktop Services licenses\. For details, see [Microsoft Licensing on AWS](http://aws.amazon.com/windows/resources/licensing/)\.