# Launch AWS Service Catalog products with Terraform<a name="launch-wizard-sap-service-catalog-terraform"></a>

The official HashiCorp AWS provider supports AWS Service Catalog resources\. You can launch products created with Launch Wizard and saved to AWS Service Catalog using Terraform\. Or, you can integrate the products with their existing Terraform workflows\. Administrators can create AWS Service Catalog portfolios and add Launch Wizard products to them using Terraform\.

**Prerequisites for using Terraform to launch products:**
+ You must create a deployment using Launch Wizard by choosing the **Create an AWS Service Catalog product** option in the infrastructure settings in Launch Wizard\. For more information, see [Define infrastructure](launch-wizard-sap-deploying.md#launch-wizard-sap-infrastructure)\.
+ The Terraform user that authenticates the AWS account must have access to the AWS Service Catalog products\. For more information, see [AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) in the Terraform documentation\.
+ The IAM user that authenticates the AWS account must have permissions to use the AWS Service Catalog products created by Launch Wizard\. For steps to grant access to users, see [Granting Access to Users](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/catalogs_portfolios_users.html) in the *AWS Service Catalog User Guide*\.

The Terraform resource named [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/servicecatalog_provisioned_product](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/servicecatalog_provisioned_product) is used to launch the AWS Service Catalog product created with Launch Wizard\.

**Example Terraform script**

The following example Terraform script launches a single node HANA database instance with a single node HANA product \(`prod-abc1234546`\) created with Launch Wizard using the product version ID \(`pa-xyz12345`\)\. In this example, the hostname for HANA and the SID for HANA DB are passed to override the defaults, and the remaining parameters are set to the defaults in the AWS Service Catalog product\.

```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.54.0"
    }
  }
}
provider "aws" {
  profile = "default"
  region  = "us-east-1"
}
resource "random_id" "id" {
  byte_length = 8
}
#Confirm user can launch product  - No launch paths has many reasons for failure.
resource "aws_servicecatalog_provisioned_product" "singlenodehana" {
  name = "tef-${random_id.id.hex}"
  product_id = "prod-abc1234546"
  provisioning_artifact_id = "pa-xyz12345"
  provisioning_parameters {
        key = "HANASID"
        value = "HDB"
  }
  provisioning_parameters {
        key = "HANAHostname"
        value = "saphanadev"    
  }
tags = {
    TFLaunched= "True"
  }
}
```

Note that the environment variables authentication mechanism is used in this example\.