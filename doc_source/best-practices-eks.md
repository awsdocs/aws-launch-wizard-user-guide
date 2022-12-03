# Best practices<a name="best-practices-eks"></a>

Best practices for using Amazon EKS on AWS

**Topics**
+ [Amazon EKS application best practices](#amazon-eks-application-best-practices)
+ [Use AWS CloudFormation for ongoing management](#use-cloudformation)
+ [Monitor additional resource usage](#monitor-resource-usage)
+ [Security](#eks-security)

## Amazon EKS application best practices<a name="amazon-eks-application-best-practices"></a>

 For more information about best practices for your Amazon EKS application, see the [EKS Best Practices Guides](https://aws.github.io/aws-eks-best-practices/)\. 

## Use AWS CloudFormation for ongoing management<a name="use-cloudformation"></a>

We recommend using CloudFormation for managing updates and resources that are created by this Launch Wizard deployment\. Using the Amazon EC2 console, AWS CLI, or API to change or delete resources can cause future CloudFormation operations on the stack to behave unexpectedly\. 

## Monitor additional resource usage<a name="monitor-resource-usage"></a>

This deployment enables users of the Amazon EKS cluster to use Elastic Load Balancing and Amazon EBS volumes as part of their Kubernetes applications\. Because these carry additional costs, we recommend that you grant users of the Amazon EKS cluster the minimum permissions required according to Kubernetes Role Based Access Control \(RBAC\)\. We also recommend that you monitor resource usage by using the Kubernetes CLI or API to describe persistent volume claims \(PVC\) and Elastic Load Balancing resources across all namespaces\. To disable this functionality, update the `ControlPlaneRole` IAM role in the child stack to restrict access to the Kubernetes control plane for specific AWS APIs, such as `ec2:CreateVolume` and `elb:CreateLoadBalancer`\.

## Security<a name="eks-security"></a>

Amazon EKS uses IAM to authenticate your Kubernetes cluster, but it still relies on native Kubernetes RBAC\. This means that IAM is used only for valid entities\. All permissions for interacting with your Amazon EKS cluster’s Kubernetes API are managed by the native Kubernetes RBAC system\. We recommend that you grant least privilege access through Kubernetes RBAC\.