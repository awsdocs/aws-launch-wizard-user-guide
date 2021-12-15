# What is AWS Launch Wizard for Amazon Elastic Kubernetes Service?<a name="what-is-launch-wizard-eks"></a>

AWS Launch Wizard for Amazon Elastic Kubernetes Service \(Amazon EKS\) guides you through the sizing, configuration, and deployment of an Amazon EKS control plane, connecting worker nodes to the cluster, and configuring a bastion host for cluster admin operations\. Additionally, the deployment provides custom resources that enable you to deploy and manage your Kubernetes applications using AWS CloudFormation by declaring Kubernetes manifests or Helm charts directly in CloudFormation templates\.

**Topics**
+ [Deployment options](#launch-wizard-eks-deployments)
+ [Components](#launch-wizard-eks-components)

## Deployment options<a name="launch-wizard-eks-deployments"></a>

Launch Wizard for Amazon EKS supports the following deployment types:
+ Deploy an Amazon EKS cluster into a new Amazon Virtual Private Cloud in your AWS account\.
+ Deploy an Amazon EKS cluster into an existing Amazon Virtual Private Cloud in your AWS account\.

## Components<a name="launch-wizard-eks-components"></a>

An Amazon EKS environment deployed with Launch Wizard will include the following components:
+ A highly available architecture that spans three Availability Zones\.
+ In one public subnet, a Linux bastion host in an Auto Scaling group to allow inbound Secure Shell \(SSH\) access to Amazon Elastic Compute Cloud \(Amazon EC2\) instances in private subnets\. The bastion host is also configured with the Kubernetes `kubectl` command line interface \(CLI\) for managing the Kubernetes cluster\.
+ An Amazon EKS cluster, which creates the Kubernetes control plane\.
+ In the private subnets, a group of Kubernetes nodes\.
+ Resource Groups that contain all the resources created with Launch Wizard\.

Additionally, a new VPC deployment includes the following components:
+ A VPC configured with public and private subnets according to AWS best practices, to provide you with your own virtual network in AWS\.
+ In the public subnets, managed NAT gateways to allow outbound internet access for resources in the private subnets\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/amazon-eks-on-aws-architecture-diagram.png)