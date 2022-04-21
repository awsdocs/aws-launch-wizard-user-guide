# Launch AWS Service Catalog products created with AWS Launch Wizard<a name="launch-wizard-sap-service-catalog"></a>

This section contains information to help you set up for and access AWS Service Catalog products created with AWS Launch Wizard to launch those products\. It also contains information about how to create a launch constraint so that you don't have to use your own IAM credentials to launch and manage AWS Service Catalog products\.

**Topics**
+ [Set up to launch AWS Service Catalog products created with AWS Launch Wizard](#launch-wizard-sap-service-catalog-setup)
+ [Create a launch constraint](#launch-wizard-sap-service-catalog-constraint)
+ [Access AWS Service Catalog products created with AWS Launch Wizard](#launch-wizard-sap-service-catalog-access)
+ [AWS Service Catalog deployment errors](#launch-wizard-sap-service-catalog-errors)

## Set up to launch AWS Service Catalog products created with AWS Launch Wizard<a name="launch-wizard-sap-service-catalog-setup"></a>

This section provides the required steps to grant permissions to the user group\. This requirement must be met to access AWS Service Catalog products created with Launch Wizard to launch those products\.

**Grant AWS Service Catalog permissions to the user group**

1. Navigate to the [AWS Identity and Access Management console](https://console.aws.amazon.com/iam)\.

1. Choose **User groups** from the left navigation pane\.

1. Choose **Create group\.**

1. For **User group name**, enter `Endusers`\. 

1. Enter `AWSServiceCatalog` in the search box to filter the policy list\.

1. Select the check box next to the **AWSServiceCatalogEndUserFullAccess** policy\. You can optionally choose **AWSServiceCatalogEndUserReadOnlyAccess** if you prefer to grant the user only read\-only access\. Choose **Create group**

1. To add a new user to the group, in the left navigation pane, choose **Users**\.

1. Choose **Add user**\.

1. Enter a **User name**\.

1. Select **AWS Management Console access**\.

1. Choose **Next: Permissions**\.

1. Choose **Add user to group**\.

1. Select the check box next to the **Endusers** group, then choose **Next:Tags**\.

1. Choose **Next: Review**\. On the **Review** page, choose **Create user**\. Download or copy the credentials, then choose **Close**\.

## Create a launch constraint<a name="launch-wizard-sap-service-catalog-constraint"></a>

A launch constraint specifies the AWS Identity and Access Management role that AWS Service Catalog assumes when a user launches a product\. It is associated with products in the portfolio\. If you do not use launch constraints, you must launch and manage products using your own IAM credentials\. These credentials must have permissions to use AWS CloudFormation, AWS Service Catalog, and any other AWS services used by the products\. Using a launch constraint allows you to limit the permissions of a user to the minimum required for a product\.

To create a launch constraint, complete the steps in the following procedure\. Perform Step 2 for each of the following listed policies\.

**Create the launch role**

### AWS Service Catalog launch constraint policy 1<a name="launch-constraint-policy-1"></a>

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

### Service Catalog launch constraint policy 2<a name="launch-constraint-policy-2"></a>

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

### Service Catalog launch constraint policy 3<a name="launch-constraint-policy-3"></a>

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

1. Perform the following substeps individually for each of the three policies previously listed\.

   1. In the left navigation pane, choose **Policies** > **Create policy**\.

   1. On the **Create policy** page, choose the **JSON** tab\.

   1. Copy each of the previous policies and paste each into the **Policy Document** JSON text box, replacing the placeholder text\. 

   1. Choose **Next: Tags** > **Next: Review**\.

   1. Enter a **Policy Name**\.

   1. Choose **Create policy**\.

1. In the left navigation pane, choose **Roles**, then choose **Create role**\.

1. Under **Select type of trusted entity**, choose **AWS service** > **Service Catalog**\.

1. Select the **Service Catalog** use case, then choose **Next:Permissions**\.

1. Search for the three policies that you added in Step 2 and select the check boxes next to them\.

1. Choose **Next: Tags**\.

1. Choose **Next: Review**\.

1. Enter `LaunchWizardServiceCatalogProductsLaunchRole` for the **Role name**\.

1. Choose **Create role**\.

**Create launch constraint**

1. Navigate to the [AWS Service Catalog console](https://console.aws.amazon.com/servicecatalog)\.

1. In the left navigation pane, under **Administration**, choose **Portfolios**\.

1. Choose the portfolio named **Launch Wizard Service Catalog portfolio**, which is the default portfolio\.

1. Under **Constraints**, choose **Create Constraints**\.

1. Select the **Product** to which to apply the constraint\.

1. Select **Launch** as the **Constraint type**\.

1. Select the IAM role that you created in the procedure for creating a launch role\.

1. Choose **Create**\.

## Access AWS Service Catalog products created with AWS Launch Wizard<a name="launch-wizard-sap-service-catalog-access"></a>

Perform the following steps to access AWS Service Catalog products created with AWS Launch Wizard\.

In the AWS Service Catalog administrator console, the **Portfolio details** page lists the portfolio settings\. From this page, you can manage the products in a portfolio, grant users access to products, and apply `TagOptions` and constraints\. You can manage products from the **Products** page\.

**Access Service Catalog products as a Service Catalog Admin user**

1. Navigate to the [AWS Service Catalog console](https://console.aws.amazon.com/servicecatalog)\.

1. In the left navigation pane, under **Administration**, choose **Portfolios**\.

1. Choose the portfolio named **AWS Launch Wizard Products**, which is the default portfolio created by Launch Wizard\.

1. Choose **AWS Launch Wizard products**\.

1. The product created by Launch Wizard using AWS CloudFormation templates and user inputs is named **\[LW Deployment Name\]\-\[Deployment Type\]**\. You can create a new version by choosing **Create new version**\.

1. You can associate tags or apply product\-specific tags as needed\.

**Access Service Catalog products as an IAM user**

1. Navigate to the [AWS Service Catalog console](https://console.aws.amazon.com/servicecatalog)\.

1. In the left navigation pane\. under **Home**, choose **Products**\.

1. Search for the Launch Wizard SAP product that you saved from the Launch Wizard deployment, and select it\. The product, won't be visible to any user who has not been granted access to it\. To grant access to the product, see [Granting Access to Users](https://docs.aws.amazon.com/ervicecatalog/latest/adminguide/catalogs_portfolios_users.html) in the *AWS Service Catalog User Guide*\. 

1. Choose **Launch product**\.

1. You will be directed to the AWS Service Catalog **Launching** page, which resembles AWS CloudFormation\. Most of the parameters are specified using your defaults\. Enter or replace the default values as you require, including passwords and SAPSIDs\.

1. After you verify the parameters, choose **Launch product** to start the creation of the AWS CloudFormation stack\.

## AWS Service Catalog deployment errors<a name="launch-wizard-sap-service-catalog-errors"></a>

For AWS Service Catalog deployments completed prior to February 7, 2022, perform the following steps to remove the `AmazonLambdaRolePolicyForLaunchWizardSAP` policy from the `AmazonLambdaRoleForLaunchWizard` role, and add a new inline policy\. Deployments completed after February 7, 2022 do not require you to perform these steps\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Roles** from the left navigation pane\.

1. Search for the `AmazonLambdaRoleForLaunchWizard`\. Select the policy to view the attached permissions\.

1. Check whether the `AmazonLambdaRolePolicyForLaunchWizardSAP` policy is attached to this role\. If it is attached, remove the policy by selecting the check box next to it, and choose **Remove**\.

1. Add the following inline policy by choosing **Add permissions**>**Create inline policy**, and entering the policy in the **JSON** tab of the **Create policy** wizard\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "ssm:GetParameter"
         ],
         "Resource": "arn:aws:ssm:::parameter/LaunchWizard*"
       },
       {
         "Effect": "Allow",
         "Action": [
           "ssm:GetDocument",
           "ssm:sendCommand"
         ],
         "Resource": [
           "arn:aws:ssm:::document/AWS-RunShellScript"
         ]
       },
       {
         "Effect": "Allow",
         "Action": [
           "ssm:SendCommand"
         ],
         "Resource": [
           "arn:aws:ec2:::instance/*"
         ],
         "Condition": {
           "StringLike": {
             "ssm:resourceTag/LaunchWizardApplicationType": "*"
           }
         }
       }
     ]
   }
   ```

1. Choose **Review policy**, enter a name for the policy, and choose **Create policy**\.