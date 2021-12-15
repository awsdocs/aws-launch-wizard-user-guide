# Launch AWS CloudFormation templates created in Launch Wizard<a name="launch-wizard-sap-launch-artifacts-cloudformation"></a>

You can launch AWS CloudFormation stacks from the AWS CloudFormation templates that you saved from your successful Launch Wizard deployments\. Perform the following steps to find and launch your AWS CloudFormation templates created with Launch Wizard\.

To create a launch constraint, complete the steps in the following procedure\. Perform Step 2 for each of the following listed policies\.

**Attach required policies to IAM user**

## Service Catalog launch constraint policy 1<a name="launch-constraint-policy-1"></a>

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
                "ds:DeleteDirectory"
            ],
            "Resource": "*"
        }
    ]
}
```

## Service Catalog launch constraint policy 2<a name="launch-constraint-policy-2"></a>

```
{
    "Version": "2012-10-17",
    "Statement": [
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
                "arn:aws:cloudformation:*:*:stack/*/*",
                "arn:aws:cloudformation:*:*:stack/ApplicationInsights*/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:StopInstances",
                "ec2:TerminateInstances"
            ],
            "Resource": "*"
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
                "arn:aws:iam::*:instance-profile/*"
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
                "arn:aws:iam::*:instance-profile/*"
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
                "arn:aws:resource-groups:*:*:group/*",
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
            "Action": "ssm:SendCommand",
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
                "cloudformation:GetTemplateSummary",
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
                "ssm:StartAutomationExecution",
                "ssm:StopAutomationExecution",
                "tag:Get*"
            ],
            "Resource": "*"
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
            "Resource": "arn:aws:sqs:*:*:*"
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
                "arn:aws:cloudwatch:*:*:alarm:*",
                "arn:aws:iam::*:instance-profile/*"
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
        }
    ]
}
```

## Service Catalog launch constraint policy 3<a name="launch-constraint-policy-3"></a>

```
{
    "Version": "2012-10-17",
    "Statement": [
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
                "arn:aws:lambda:*:*:function:*",
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
            "Resource": "arn:aws:dynamodb:*:*:table/*"
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
            "Resource": "arn:aws:secretsmanager:*:*:secret:*"
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
            "Resource": "arn:aws:sns:*:*:*"
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
                "s3:GetObject"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "s3:ExistingObjectTag/servicecatalog:provisioning": "true"
                }
            }
        }
    ]
}
```

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam)\.

1. Perform the following substeps for each of the three policies listed above\.

   1. In the left navigation pane, choose **Policies** > **Create policy**\.

   1. On the **Create policy** page, choose the **JSON** tab\.

   1. Copy each policy above and paste it into the **Policy Document** JSON text field, replacing the placeholder text \(perform these substeps individually for each of the three policies listed above\)\.

   1. Choose **Next: Tags** > **Next: Review**\.

   1. Enter a **Policy Name**\.

   1. Choose **Create policy**\.

1. Attach the three policies you just created to the IAM user you use to launch AWS CloudFormation templates\.

**Find and launch your templates**

1. Navigate to the [Amazon S3 console](https://console.aws.amazon.com/s3)\.

1. Locate the name of the location within the Amazon S3 bucket that you specified when you [defined the infrastructure for your Launch Wizard deployment](launch-wizard-sap-deploying.md#launch-wizard-sap-infrastructure)\. 

1. Under the folder that you specified, locate and choose a new folder named `<LaunchWizardDeploymentName>-<TimeStamp>`\. This is the folder to which the Launch Wizard service copies the AWS CloudFormation templates and deployment artifacts\.

1. After you choose the new folder, you will see an `sap/` folder and a JSON file named `<LaunchWizardDeploymentName>-<DeploymentType>-template.json`\. This is the root AWS CloudFormation template file\. Select the check box next to this file and choose **Copy URL**\.

1. Navigate to the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation) to create a stack with the URL that you copied\.

For more information about CloudFormation templates, see [Working with AWS CloudFormation templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html) in the *AWS CloudFormation User Guide*\.