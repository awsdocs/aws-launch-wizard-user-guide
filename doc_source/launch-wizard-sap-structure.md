# Amazon S3 folder structure for HANA software download<a name="launch-wizard-sap-structure"></a>

To download the SAP HANA software to your Amazon S3 bucket, you must set up your destination bucket\.

**Set up destination bucket**

1. Choose **Create Bucket**\.

1. In the **Create Bucket** dialog box, provide a name for your new S3 bucket with the prefix `launchwizard`\. Choose the **AWS Region** where you want to create the S3 bucket, which should be a Region that is close to your location, and then follow the wizard to configure permissions and other settings\. For detailed information about bucket names and Region selection, see [Create a Bucket ](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)in the **Amazon S3 Getting Started Guide**\.

1. Choose the bucket that you created and add folders to organize your SAP HANA downloads\. We recommend that you create a folder for each version of SAP HANA\.

 If the path for the specific version of SAP HANA software is `s3://launchwizardhanamedia/SP23` or `s3://launchwizardhanamedia/SP24`, then use this path in the Amazon S3 URL for SAP HANA software \(HANAInstallMedia\) parameter\. 

**Note**  
We recommend that you place only the main SAP HANA installation files in the S3 bucket\. Do not place multiple SAP HANA versions in the same folder\.
