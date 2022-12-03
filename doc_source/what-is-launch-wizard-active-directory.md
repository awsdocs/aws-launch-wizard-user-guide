# What is AWS Launch Wizard for Active Directory?<a name="what-is-launch-wizard-active-directory"></a>

AWS Launch Wizard for Active Directory is a service that applies [AWS cloud application best practices](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) to guide you through setting up a new Active Directory infrastructure, or adding domain controllers to an existing infrastructure either in the AWS Cloud or on premises\. The deployment environment includes various resources such as a new or existing VPC, security groups, and AWS Identity and Access Management \(IAM\) roles\. You can set up a new Active Directory infrastructure with domain controllers on Amazon EC2 instances, add domain controllers on Amazon EC2 instances to extend your existing Active Directory infrastructure, or use AWS Directory Service for Microsoft Active Directory for a managed service experience\.

Launch Wizard reduces the time that it takes to set up an Active Directory infrastructure and deploy self\-managed domain controllers to the cloud or on premises\. You input your domain controller requirements, including number of nodes and connectivity, on the service console, and AWS Launch Wizard identifies the right AWS resources to deploy your self\-managed domain controllers\. AWS Launch Wizard provides an estimated cost of deployment, and gives you the ability to modify your resources and instantly view the updated cost assessment\. When you approve, AWS Launch Wizard provisions and configures the selected resources in a few hours to create fully\-functioning, production\-ready domain controllers\. 

After you deploy your self\-managed domain controllers, they are ready to use and can be accessed on the Amazon Elastic Compute Cloud \(Amazon EC2\) console\. 

**Topics**
+ [Supported operating systems](#launch-wizard-ad-os)
+ [Features of AWS Launch Wizard](launch-wizard-ad-features.md)
+ [Components](launch-wizard-ad-components.md)
+ [Requirements](launch-wizard-ad-requirements.md)
+ [Related services](lw-ad-related-services.md)
+ [How AWS Launch Wizard works](how-launch-wizard-ad-works.md)
+ [Domain controller launch limits](#launch-wizard-ad-limits)
+ [AWS Regions](#launch-wizard-ad-regions)

## Supported operating systems<a name="launch-wizard-ad-os"></a>

AWS Launch Wizard for Active Directory supports the Windows Server 2022 operating system\.

## Domain controller launch limits<a name="launch-wizard-ad-limits"></a>

A single Launch Wizard deployment for Active Directory launches two domain controllers per each AWS Region\. If you want to add more domain controllers, you can create additional Launch Wizard for Active Directory deployments to add them to the same Active Directory infrastructure\. For more information, see [Extend on\-premises Active Directory to an existing Amazon Virtual Private Cloud](launch-wizard-ad-deploying-existing-vpc-extend.md)\.

## AWS Regions<a name="launch-wizard-ad-regions"></a>

Launch Wizard uses AWS Secrets Manager during the provisioning of self\-managed domain controllers into an environment\. These services are not supported in all AWS Regions\. For a current list of supported Regions, see the [Service endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/aws-service-information.html)\. 