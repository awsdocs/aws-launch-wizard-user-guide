# AWS managed policies for AWS Launch Wizard<a name="security-iam-awsmanpol"></a>





To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the `ViewOnlyAccess` AWS managed policy provides read\-only access to many AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.

**Topics**
+ [AmazonLaunchWizard\_Fullaccess](#security-iam-awsmanpol-AmazonLaunchWizard_Fullaccess)
+ [AmazonEC2RolePolicyForLaunchWizard](#security-iam-awsmanpol-AmazonEC2RolePolicyForLaunchWizard)
+ [Policy updates](#security-iam-awsmanpol-updates)









## AWS managed policy: AmazonLaunchWizard\_Fullaccess<a name="security-iam-awsmanpol-AmazonLaunchWizard_Fullaccess"></a>





You can attach the `AmazonLaunchWizard_Fullaccess` policy to your IAM identities\.



This policy grants administrative permissions that allow full access to AWS Launch Wizard and other required services\.



**Permissions details**

This policy includes the following permissions\.




+ `launchwizard` – Allows all Launch Wizard actions\.
+ `applicationinsights` – Allows all CloudWatch Application Insights actions\. This permission is required so that an application can be tracked and configured by CloudWatch Application Insights, which provides Launch Wizard with more visibility and insight into the service through functionality such as monitoring and data analysis\.
+ `route53` – Allows changing and listing resource record sets, listing hosted zones, and listing hosted zones by name\. This is required so that scripts running on instances in a customer's account for SAP deployments can perform these actions\.
+ `s3` – Allows all get or list operations for all resources, and allows for creation, deletion, and getting objects from a bucket, and putting objects in a bucket for certain Launch Wizard and SAP resources\. This is required so that the Launch Wizard service can both view and update buckets and contents in Amazon S3 for tasks such as reading and storing scripts that are run on instances in its deployments\.
+ `kms` – Allows listing all AWS KMS keys and aliases\. This is required so that Launch Wizard can view keys and aliases in a customer's account\.
+ `cloudwatch` – Allows all get, list, or describe actions for all resources, and allows Launch Wizard alarms and instance profiles to be created, updated, deleted, or described\. This is required so that Launch Wizard can create and manage alarms to track metrics\.
+ `ec2` – Allows creation of all security groups, authorization of ingress rules for all security groups, all get or describe operations, and creation of all VPCs, NAT/internet gateways, subnets, routes/route tables, and key pairs\. Allows instances from the AWS CloudFormation stacks in Launch Wizard deployments to be stopped or terminated\. Allows anything called from the Launch Wizard endpoint to perform other Amazon EC2 actions\. This is required so that all EC2\-related resources deployed from the Launch Wizard CloudFormation stacks can be appropriately created and managed\.
+ `cloudformation` – Allows all Launch Wizard and CloudWatch Application Insights CloudFormation stacks to be described and listed\. Allows all get operations, all resources to be signaled, and all Launch Wizard stacks to be deleted\. Allows all stacks to be created, and allows describe account limits, describe stack drift detection status, all list operations, and tagging of resources with all tag keys, starting with "Launch Wizard"\. This is required so that Launch Wizard can create CloudFormation stacks in a customer's account, so that the stacks are appropriately signaled, and so that a customer can view and delete those stacks\. 
+ `iam` – Allows Launch Wizard EC2 roles and instance profiles to be created and deleted and attached/detached\. Allows Launch Wizard EC2 and AWS Lambda roles and instance profiles to be passed a role as long as it is passed to Lambda or EC2\. Allows get operations for all roles or policies, all list operations, and all roles linked to Amazon EC2 Auto Scaling, CloudWatch Application Insights, or Amazon EventBridge to be created\. This is required so that Launch Wizard can create necessary roles/users/policies and attach the appropriate roles/policies to them to ensure that resources in the Launch Wizard CloudFormation stacks and elsewhere in the service have the appropriate permissions\.
+ `autoscaling` – Allows Launch Wizard Auto Scaling groups and launch configurations to be created, deleted, and updated\. This is required so that the Launch Wizard SQL CloudFormation stacks can perform these actions for the RDGW nodes in its deployments\.
+ `logs` – Allows all log groups or log streams to be created and deleted\. Allows all write and read log events\. This is required so that Launch Wizard can publish logs to a customer's account so that a customer can view the events from their deployments\.
+ `sns` – Allows Launch Wizard Amazon SNS topics to be created, deleted, subscribed to, and unsubscribed from\. Allows all Amazon SNS subscriptions to be listed and messages to be published\. This is required so that the Launch Wizard Amazon SNS queues to send signals between resources and Launch Wizard Lambda functions know when to proceed with steps in their event\-based workflows\.
+ `resource-groups` – Allows resource groups whose names begin with "Launch Wizard" to be created, deleted, or listed\. This is required so that Launch Wizard resources can be grouped together in a resource group, and so that the groups can be viewed or deleted\.
+ `ds` – Allows creation and deletion of a Microsoft Active Directory, adding IP routes, and all describe operations\. This is required so that Active Directories can be created, deleted, and viewed in Launch Wizard SQL Server deployments, and so that IP routes can be added to them\.
+ `sqs` – Allows all queues with "SQS" in the name to be tagged, listed, created, and deleted\. Allows any queue attributes to be set and read, and for the queue URL to be read and permissions added\. This is required so that Launch Wizard SAP deployments can have a queue in the deployment on which these actions can be performed\.
+ `elasticfilesystem` – Allows all file systems to be created, deleted, and described, and for mount targets to be created, deleted, and described\. This is required so that Launch Wizard SAP deployments can create Amazon EFS file systems in a customer's account with the appropriate mount targets\.
+ `lambda` – Allows AWS Lambda functions with "Launch Wizard" in the name to be created, deleted, read, and invoked\. This is required so that Launch Wizard SAP deployments can perform some Lambda functions at the end of CloudFormation stacks for configuration in a customer's account or for parameter validation\.
+ `dynamodb` – Allows all tables with a name starting with "Launch Wizard" to be created, deleted, or described\. This is required so that Launch Wizard scripts for SAP can publish events and metadata from the events of the running threads into a Amazon DynamoDB table in a customer's account\.
+ `secretsmanager` – Allows all secrets with a name starting with "Launch Wizard" to be created, deleted, and retrieved, all resources to be tagged or untagged, all resource policies to be created and deleted, and secret version IDs to be listed\. Allows all random passwords to be generated and all secrets to be listed\. This is required so that secrets can be created in a customer's account to perform operations, such as decrypting a password in order to RDP into an instance from their deployment\.
+ `fsx` – Allows Amazon FSx file systems to be created by all resources\. Allows describing file system properties, listing all tags on the Amazon FSx file share, adding and removing tags, and deleting file systems where tags include `LaunchWizard` in the name\.
+ `servicecatalog` – Allows for the creation of an AWS Service Catalog portfolio and product\. Allows for the creation of a LaunchConstraint\. Allows for the association between product and portfolio, as well as for the association between the IAM principal of a user and a portfolio\.
+ `ssm` – Allows for all get, list, tag, execute, and delete operations for all SSM resources\. This is required so that Launch Wizard can create, run, and delete SSM resources on behalf of the customer to configure their Amazon EC2 instances for application provisioning\. 



```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "applicationinsights:*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "resource-groups:List*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "route53:ChangeResourceRecordSets",
        "route53:GetChange",
        "route53:ListResourceRecordSets",
        "route53:ListHostedZones",
        "route53:ListHostedZonesByName"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListAllMyBuckets",
        "s3:ListBucket",
        "s3:GetBucketLocation"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "kms:ListKeys",
        "kms:ListAliases"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudwatch:List*",
        "cloudwatch:Get*",
        "cloudwatch:Describe*"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:CreateInternetGateway",
        "ec2:CreateNatGateway",
        "ec2:CreateVpc",
        "ec2:CreateKeyPair",
        "ec2:CreateRoute",
        "ec2:CreateRouteTable",
        "ec2:CreateSubnet"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:AllocateAddress",
        "ec2:AllocateHosts",
        "ec2:AssignPrivateIpAddresses",
        "ec2:AssociateAddress",
        "ec2:CreateDhcpOptions",
        "ec2:CreateEgressOnlyInternetGateway",
        "ec2:CreateNetworkInterface",
        "ec2:CreateVolume",
        "ec2:CreateVpcEndpoint",
        "ec2:CreateTags",
        "ec2:DeleteTags",
        "ec2:RunInstances",
        "ec2:StartInstances",
        "ec2:ModifyInstanceAttribute",
        "ec2:ModifySubnetAttribute",
        "ec2:ModifyVolumeAttribute",
        "ec2:ModifyVpcAttribute",
        "ec2:AssociateDhcpOptions",
        "ec2:AssociateSubnetCidrBlock",
        "ec2:AttachInternetGateway",
        "ec2:AttachNetworkInterface",
        "ec2:AttachVolume",
        "ec2:DeleteDhcpOptions",
        "ec2:DeleteInternetGateway",
        "ec2:DeleteKeyPair",
        "ec2:DeleteNatGateway",
        "ec2:DeleteSecurityGroup",
        "ec2:DeleteVolume",
        "ec2:DeleteVpc",
        "ec2:DetachInternetGateway",
        "ec2:DetachVolume",
        "ec2:DeleteSnapshot",
        "ec2:AssociateRouteTable",
        "ec2:AssociateVpcCidrBlock",
        "ec2:DeleteNetworkAcl",
        "ec2:DeleteNetworkInterface",
        "ec2:DeleteNetworkInterfacePermission",
        "ec2:DeleteRoute",
        "ec2:DeleteRouteTable",
        "ec2:DeleteSubnet",
        "ec2:DetachNetworkInterface",
        "ec2:DisassociateAddress",
        "ec2:DisassociateVpcCidrBlock",
        "ec2:GetLaunchTemplateData",
        "ec2:ModifyNetworkInterfaceAttribute",
        "ec2:ModifyVolume",
        "ec2:AuthorizeSecurityGroupEgress",
        "ec2:GetConsoleOutput",
        "ec2:GetPasswordData",
        "ec2:ReleaseAddress",
        "ec2:ReplaceRoute",
        "ec2:ReplaceRouteTableAssociation",
        "ec2:RevokeSecurityGroupEgress",
        "ec2:RevokeSecurityGroupIngress",
        "ec2:DisassociateIamInstanceProfile",
        "ec2:DisassociateRouteTable",
        "ec2:DisassociateSubnetCidrBlock",
        "ec2:ModifyInstancePlacement",
        "ec2:DeletePlacementGroup",
        "ec2:CreatePlacementGroup",
        "elasticfilesystem:DeleteFileSystem",
        "elasticfilesystem:DeleteMountTarget",
        "ds:AddIpRoutes",
        "ds:CreateComputer",
        "ds:CreateMicrosoftAD",
        "ds:DeleteDirectory",
        "servicecatalog:AssociateProductWithPortfolio",
        "cloudformation:GetTemplateSummary",
        "sts:GetCallerIdentity"
      ],
      "Resource": "*",
      "Condition": {
        "ForAnyValue:StringEquals": {
          "aws:CalledVia": "launchwizard.amazonaws.com"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:DescribeStack*",
        "cloudformation:Get*",
        "cloudformation:ListStacks",
        "cloudformation:SignalResource",
        "cloudformation:DeleteStack"
      ],
      "Resource": [
        "arn:aws:cloudformation:*:*:stack/LaunchWizard*/*",
        "arn:aws:cloudformation:*:*:stack/ApplicationInsights*/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:StopInstances",
        "ec2:TerminateInstances"
      ],
      "Resource": "*",
      "Condition": {
        "StringLike": {
          "ec2:ResourceTag/aws:cloudformation:stack-id": "arn:aws:cloudformation:*:*:stack/LaunchWizard-*/*"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:CreateInstanceProfile",
        "iam:DeleteInstanceProfile",
        "iam:RemoveRoleFromInstanceProfile",
        "iam:AddRoleToInstanceProfile"
      ],
      "Resource": [
        "arn:aws:iam::*:role/service-role/AmazonEC2RoleForLaunchWizard*",
        "arn:aws:iam::*:instance-profile/LaunchWizard*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:PassRole"
      ],
      "Resource": [
        "arn:aws:iam::*:role/service-role/AmazonEC2RoleForLaunchWizard*",
        "arn:aws:iam::*:role/service-role/AmazonLambdaRoleForLaunchWizard*",
        "arn:aws:iam::*:instance-profile/LaunchWizard*"
      ],
      "Condition": {
        "StringEqualsIfExists": {
          "iam:PassedToService": [
            "lambda.amazonaws.com",
            "ec2.amazonaws.com"
          ]
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "autoscaling:AttachInstances",
        "autoscaling:CreateAutoScalingGroup",
        "autoscaling:CreateLaunchConfiguration",
        "autoscaling:DeleteAutoScalingGroup",
        "autoscaling:DeleteLaunchConfiguration",
        "autoscaling:UpdateAutoScalingGroup",
        "logs:CreateLogStream",
        "logs:DeleteLogGroup",
        "logs:DeleteLogStream",
        "logs:DescribeLog*",
        "logs:PutLogEvents",
        "resource-groups:CreateGroup",
        "resource-groups:DeleteGroup",
        "sns:ListSubscriptionsByTopic",
        "sns:Publish",
        "ssm:DeleteDocument",
        "ssm:DeleteParameter*",
        "ssm:DescribeDocument*",
        "ssm:GetDocument",
        "ssm:PutParameter"
      ],
      "Resource": [
        "arn:aws:resource-groups:*:*:group/LaunchWizard*",
        "arn:aws:sns:*:*:*",
        "arn:aws:autoscaling:*:*:autoScalingGroup:*:autoScalingGroupName/LaunchWizard*",
        "arn:aws:autoscaling:*:*:launchConfiguration:*:launchConfigurationName/LaunchWizard*",
        "arn:aws:ssm:*:*:parameter/LaunchWizard*",
        "arn:aws:ssm:*:*:document/LaunchWizard*",
        "arn:aws:logs:*:*:log-group:*:*:*",
        "arn:aws:logs:*:*:log-group:LaunchWizard*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "ssm:GetDocument",
        "ssm:SendCommand"
      ],
      "Resource": [
        "arn:aws:ssm:*::document/AWS-RunShellScript"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "ssm:SendCommand"
      ],
      "Resource": [
        "arn:aws:ec2:*:*:instance/*"
      ],
      "Condition": {
        "StringLike": {
          "aws:ResourceTag/aws:cloudformation:stack-id": "arn:aws:cloudformation:*:*:stack/LaunchWizard-*/*"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "logs:DeleteLogStream",
        "logs:GetLogEvents",
        "logs:PutLogEvents",
        "ssm:AddTagsToResource",
        "ssm:DescribeDocument",
        "ssm:GetDocument",
        "ssm:ListTagsForResource",
        "ssm:RemoveTagsFromResource"
      ],
      "Resource": [
        "arn:aws:logs:*:*:log-group:*:*:*",
        "arn:aws:logs:*:*:log-group:LaunchWizard*",
        "arn:aws:ssm:*:*:parameter/LaunchWizard*",
        "arn:aws:ssm:*:*:document/LaunchWizard*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "autoscaling:Describe*",
        "cloudformation:DescribeAccountLimits",
        "cloudformation:DescribeStackDriftDetectionStatus",
        "cloudformation:List*",
        "cloudformation:ValidateTemplate",
        "ds:Describe*",
        "ds:ListAuthorizedApplications",
        "ec2:Describe*",
        "ec2:Get*",
        "iam:GetRole",
        "iam:GetRolePolicy",
        "iam:GetUser",
        "iam:GetPolicyVersion",
        "iam:GetPolicy",
        "iam:List*",
        "logs:CreateLogGroup",
        "logs:GetLogDelivery",
        "logs:GetLogRecord",
        "logs:ListLogDeliveries",
        "resource-groups:Get*",
        "resource-groups:List*",
        "servicequotas:GetServiceQuota",
        "servicequotas:ListServiceQuotas",
        "sns:ListSubscriptions",
        "sns:ListTopics",
        "ssm:CreateDocument",
        "ssm:DescribeAutomation*",
        "ssm:DescribeInstanceInformation",
        "ssm:DescribeParameters",
        "ssm:GetAutomationExecution",
        "ssm:GetCommandInvocation",
        "ssm:GetParameter*",
        "ssm:GetConnectionStatus",
        "ssm:ListCommand*",
        "ssm:ListDocument*",
        "ssm:ListInstanceAssociations",
        "ssm:SendAutomationSignal",
        "tag:Get*"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ssm:StartAutomationExecution",
        "ssm:StopAutomationExecution"
      ],
      "Resource": "arn:aws:ssm:*:*:automation-definition/LaunchWizard-*:*",
      "Condition": {
        "ForAnyValue:StringEquals": {
          "aws:CalledVia": "launchwizard.amazonaws.com"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": "logs:GetLog*",
      "Resource": [
        "arn:aws:logs:*:*:log-group:*:*:*",
        "arn:aws:logs:*:*:log-group:LaunchWizard*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:List*",
        "cloudformation:Describe*"
      ],
      "Resource": "arn:aws:cloudformation:*:*:stack/LaunchWizard*/"
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:CreateServiceLinkedRole"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "iam:AWSServiceName": [
            "autoscaling.amazonaws.com",
            "application-insights.amazonaws.com",
            "events.amazonaws.com"
          ]
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": "launchwizard:*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "sqs:TagQueue",
        "sqs:GetQueueUrl",
        "sqs:AddPermission",
        "sqs:ListQueues",
        "sqs:DeleteQueue",
        "sqs:GetQueueAttributes",
        "sqs:ListQueueTags",
        "sqs:CreateQueue",
        "sqs:SetQueueAttributes"
      ],
      "Resource": "arn:aws:sqs:*:*:LaunchWizard*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudwatch:PutMetricAlarm",
        "iam:GetInstanceProfile",
        "cloudwatch:DeleteAlarms",
        "cloudwatch:DescribeAlarms"
      ],
      "Resource": [
        "arn:aws:cloudwatch:*:*:alarm:LaunchWizard*",
        "arn:aws:iam::*:instance-profile/LaunchWizard*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:CreateStack",
        "route53:ListHostedZones",
        "ec2:CreateSecurityGroup",
        "ec2:AuthorizeSecurityGroupIngress",
        "elasticfilesystem:DescribeFileSystems",
        "elasticfilesystem:CreateFileSystem",
        "elasticfilesystem:CreateMountTarget",
        "elasticfilesystem:DescribeMountTargets",
        "elasticfilesystem:DescribeMountTargetSecurityGroups"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::launchwizard*",
        "arn:aws:s3:::launchwizard*/*",
        "arn:aws:s3:::aws-sap-data-provider/config.properties"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "cloudformation:TagResource",
      "Resource": "*",
      "Condition": {
        "ForAllValues:StringLike": {
          "aws:TagKeys": "LaunchWizard*"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:CreateBucket",
        "s3:PutBucketVersioning",
        "s3:DeleteBucket",
        "lambda:CreateFunction",
        "lambda:DeleteFunction",
        "lambda:GetFunction",
        "lambda:GetFunctionConfiguration",
        "lambda:InvokeFunction"
      ],
      "Resource": [
        "arn:aws:lambda:*:*:function:LaunchWizard*",
        "arn:aws:s3:::launchwizard*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:CreateTable",
        "dynamodb:DescribeTable",
        "dynamodb:DeleteTable"
      ],
      "Resource": "arn:aws:dynamodb:*:*:table/LaunchWizard*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "secretsmanager:CreateSecret",
        "secretsmanager:DeleteSecret",
        "secretsmanager:TagResource",
        "secretsmanager:UntagResource",
        "secretsmanager:PutResourcePolicy",
        "secretsmanager:DeleteResourcePolicy",
        "secretsmanager:ListSecretVersionIds",
        "secretsmanager:GetSecretValue"
      ],
      "Resource": "arn:aws:secretsmanager:*:*:secret:LaunchWizard*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetRandomPassword",
        "secretsmanager:ListSecrets"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ssm:CreateOpsMetadata"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "ssm:DeleteOpsMetadata",
      "Resource": "arn:aws:ssm:*:*:opsmetadata/aws/ssm/LaunchWizard*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "sns:CreateTopic",
        "sns:DeleteTopic",
        "sns:Subscribe",
        "sns:Unsubscribe"
      ],
      "Resource": "arn:aws:sns:*:*:LaunchWizard*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "fsx:UntagResource",
        "fsx:TagResource",
        "fsx:DeleteFileSystem",
        "fsx:ListTagsForResource"
      ],
      "Resource": "*",
      "Condition": {
        "StringLike": {
          "aws:ResourceTag/Name": "LaunchWizard*"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "fsx:CreateFileSystem"
      ],
      "Resource": "*",
      "Condition": {
        "StringLike": {
          "aws:RequestTag/Name": [
            "LaunchWizard*"
          ]
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "fsx:DescribeFileSystems"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "servicecatalog:CreatePortfolio",
        "servicecatalog:DescribePortfolio",
        "servicecatalog:CreateConstraint",
        "servicecatalog:CreateProduct",
        "servicecatalog:AssociatePrincipalWithPortfolio",
        "servicecatalog:CreateProvisioningArtifact"
      ],
      "Resource": [
        "arn:aws:servicecatalog:*:*:*/*",
        "arn:aws:catalog:*:*:*/*"
      ],
      "Condition": {
        "ForAnyValue:StringEquals": {
          "aws:CalledVia": "launchwizard.amazonaws.com"
        }
      }
    }
  ]
}
```

**Note**  
`arn:aws:s3:::launchwizard*` and `“arn:aws:s3:::launchwizard*/*` are redundant permissions\. Both permissions are present for historical purposes and do not impact security\.

## AWS managed policy: AmazonEC2RolePolicyForLaunchWizard<a name="security-iam-awsmanpol-AmazonEC2RolePolicyForLaunchWizard"></a>





You can attach the `AmazonEC2RolePolicyForLaunchWizard` policy to your IAM identities\.



This policy grants administrative permissions that allow all AWS Launch Wizard actions to be performed\.



**Permissions details**

This policy includes the following permissions\.




+ `launchwizard` – Allows all Launch Wizard actions\.
+ `ec2` – Allows starting, stopping, and rebooting instances, and attaching volumes to all instances with the `LaunchWizardResourceGroupID` tag\. Allows replacing route table for all instances with the `LaunchWizardApplicationType` resource tag\. Allows all resources to describe and associate IP addresses, describe instances, images, Regions, volumes, and route tables, and modify instance attributes for all resources\. Allows creating tags and volumes for all resources with the `LaunchWizardResourceType` or `LaunchWizardResourceGroupID` tags\.
+ `cloudwatch` – Allows for getting and writing metrics to CloudWatch\. This is required so that CloudWatch can write logs for all resources\.
+ `s3` – Allows all get or list operations for all resources, and allows for creation, deletion, and getting objects from a bucket, and putting objects in a bucket for certain Launch Wizard and SAP resources\. This is required so that the Launch Wizard service can both view and update buckets and contents in Amazon S3 for tasks such as reading and storing scripts that are run on instances in its deployments\.
+ `ssm` – Allows send commands to all Amazon EC2 instances with the `LaunchWizardApplicationType` resource tag\. Allows getting a document\. These actions are required to run the Backint install agent SSM document for SAP\.
+ `logs` – Allows all log groups or log streams for all write and read log events\. This is required so that Launch Wizard can publish logs to a customer's account so that a customer can view the events from their deployments\.
+ `cloudformation` – Allows all Launch Wizard and CloudWatch Application Insights CloudFormation stacks to be described and listed\. Allows all get operations and for all resources to be signaled\. This is required so that the stacks are appropriately signaled by CloudFormation\. 
+ `dynamodb` – Allows all tables with a name starting with "Launch Wizard" to be created, deleted, or described\. This is required so that Launch Wizard scripts for SAP can publish events and metadata from the events of the running threads into a Amazon DynamoDB table in a customer's account\.
+ `sqs` – Allows sending and receiving messages from Amazon SQS queues\. This is required so that Launch Wizard SAP deployments can have a queue in the deployment on which these actions can be performed\.
+ `iam` – Allows Launch Wizard EC2 roles and instance profiles to be created and deleted and attached/detached\. Allows Launch Wizard EC2 and AWS Lambda roles and instance profiles to be passed a role as long as it is passed to Lambda or EC2\. Allows get operations for all roles or policies, all list operations, and all roles linked to Amazon EC2 Auto Scaling, CloudWatch Application Insights, or Amazon EventBridge to be created\. This is required so that Launch Wizard can create necessary roles/users/policies and attach the appropriate roles/policies to them to ensure that resources in the Launch Wizard CloudFormation stacks and elsewhere in the service have the appropriate permissions\.
+ `fsx` – Allows describing file systems and listing tags on file systems on any Amazon FSx resource tagged with the `LaunchWizard` tag\. This is required so that Launch Wizard can retrieve the FSX DNS and administration endpoints to create the FCI SQL cluster\.



```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:AttachVolume",
        "ec2:RebootInstances",
        "ec2:StartInstances",
        "ec2:StopInstances"
      ],
      "Resource": [
        "arn:aws:ec2:*:*:volume/*",
        "arn:aws:ec2:*:*:instance/*"
      ],
      "Condition": {
        "StringLike": {
          "ec2:ResourceTag/LaunchWizardResourceGroupID": "*"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:ReplaceRoute"
      ],
      "Resource": "arn:aws:ec2:*:*:route-table/*",
      "Condition": {
        "StringLike": {
          "ec2:ResourceTag/LaunchWizardApplicationType": "*"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeAddresses",
        "ec2:AssociateAddress",
        "ec2:DescribeInstances",
        "ec2:DescribeImages",
        "ec2:DescribeRegions",
        "ec2:DescribeVolumes",
        "ec2:DescribeRouteTables",
        "ec2:ModifyInstanceAttribute",
        "cloudwatch:GetMetricStatistics",
        "cloudwatch:PutMetricData",
        "ssm:GetCommandInvocation"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:CreateTags",
        "ec2:CreateVolume"
      ],
      "Resource": "arn:aws:ec2:*:*:volume/*",
      "Condition": {
        "ForAllValues:StringEquals": {
          "aws:TagKeys": [
            "LaunchWizardResourceGroupID",
            "LaunchWizardApplicationType"
          ]
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket",
        "s3:PutObject",
        "s3:PutObjectTagging",
        "s3:GetBucketLocation",
        "logs:PutLogEvents",
        "logs:DescribeLogGroups",
        "logs:DescribeLogStreams"
      ],
      "Resource": [
        "arn:aws:logs:*:*:*",
        "arn:aws:s3:::launchwizard*",
        "arn:aws:s3:::aws-sap-data-provider/config.properties"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "logs:Create*",
      "Resource": "arn:aws:logs:*:*:*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*",
        "cloudformation:DescribeStackResources",
        "cloudformation:SignalResource",
        "cloudformation:DescribeStackResource",
        "cloudformation:DescribeStacks"
      ],
      "Resource": "*",
      "Condition": {
        "ForAllValues:StringEquals": {
          "aws:TagKeys": "LaunchWizardResourceGroupID"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:BatchGetItem",
        "dynamodb:PutItem",
        "sqs:ReceiveMessage",
        "sqs:SendMessage",
        "dynamodb:Scan",
        "s3:ListBucket",
        "dynamodb:Query",
        "dynamodb:UpdateItem",
        "dynamodb:DeleteTable",
        "dynamodb:CreateTable",
        "s3:GetObject",
        "dynamodb:DescribeTable",
        "s3:GetBucketLocation",
        "dynamodb:UpdateTable"
      ],
      "Resource": [
        "arn:aws:s3:::launchwizard*",
        "arn:aws:dynamodb:*:*:table/LaunchWizard*",
        "arn:aws:sqs:*:*:LaunchWizard*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "ssm:SendCommand",
      "Resource": "arn:aws:ec2:*:*:instance/*",
      "Condition": {
        "StringLike": {
          "ssm:resourceTag/LaunchWizardApplicationType": "*"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "ssm:SendCommand",
        "ssm:GetDocument"
      ],
      "Resource": [
        "arn:aws:ssm:*:*:document/AWSSAP-InstallBackint"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "fsx:DescribeFileSystems",
        "fsx:ListTagsForResource"
      ],
      "Resource": "*",
      "Condition": {
        "ForAllValues:StringLike": {
          "aws:TagKeys": "LaunchWizard*"
        }
      }
    }
  ]
}
```





## AWS Launch Wizard updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>



View details about updates to AWS managed policies for AWS Launch Wizard since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the AWS Launch Wizard Document history page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  [AmazonLaunchWizard\_Fullaccess](#security-iam-awsmanpol-AmazonLaunchWizard_Fullaccess) – Update to an existing policy  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/security-iam-awsmanpol.html)  | April 12, 2022 | 
|  [AmazonLaunchWizard\_Fullaccess](#security-iam-awsmanpol-AmazonLaunchWizard_Fullaccess) – Update to an existing policy  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/security-iam-awsmanpol.html)  | February 9, 2022 | 
|  **AmazonLambdaRoleForLaunchWizard** – Policy deprecation  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/security-iam-awsmanpol.html)  | February 7, 2022 | 
|  [AmazonEC2RolePolicyForLaunchWizard](#security-iam-awsmanpol-AmazonEC2RolePolicyForLaunchWizard) – Update to an existing policy  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/security-iam-awsmanpol.html)  | February 7, 2022 | 
|  [AmazonLaunchWizard\_Fullaccess](#security-iam-awsmanpol-AmazonLaunchWizard_Fullaccess) – Update to an existing policy  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/security-iam-awsmanpol.html)  | August 30, 2021 | 
|  [AmazonEC2RolePolicyForLaunchWizard](#security-iam-awsmanpol-AmazonEC2RolePolicyForLaunchWizard) – Update to an existing policy  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/security-iam-awsmanpol.html)  | May 21 2021 | 
|  [AmazonLaunchWizard\_Fullaccess](#security-iam-awsmanpol-AmazonLaunchWizard_Fullaccess) – Update to an existing policy  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/security-iam-awsmanpol.html)  | April 30, 2021 | 
|  AWS Launch Wizard started tracking changes  |  AWS Launch Wizard started tracking changes for its AWS managed policies\.  | April 30, 2021 | 