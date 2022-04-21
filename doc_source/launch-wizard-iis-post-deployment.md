# Post\-deployment steps<a name="launch-wizard-iis-post-deployment"></a>

The following are post\-deployment steps for Internet Information Services \(IIS\) with AWS Launch Wizard\.

**Topics**
+ [\(Optional\) Run Windows Updates](#launch-wizard-iis-updates)
+ [Testing the deployment](#launch-wizard-iis-testing)
+ [Connect to your Windows instances using SSM port forwarding sessions and RDP](#launch-wizard-iis-connect-ssm.title)

## \(Optional\) Run Windows Updates<a name="launch-wizard-iis-updates"></a>

To help ensure that the deployed servers' operating systems and installed applications have the latest Microsoft updates, run Windows Update on each server\.

### Run Windows Updates on your RD Gateways using public IP addresses<a name="launch-wizard-iis-updates-bastion.title"></a>

To run Windows updates on the RD Gateways with their public IP addresses:

1. Identify the public IP addresses for the RD Gateways, from the [Amazon EC2 Console](https://console.aws.amazon.com/ec2/)\.

1. Use the public IP of the RD Gateway to [connect to the instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html)\.

1. On the taskbar, open the **Start** menu, and choose **Settings**\.

1. In the Settings application, choose **Update & Security**

1. Choose **Check for updates**\.

1. Install any updates, and restart if necessary\.

### Run Windows Updates on your IIS servers by connecting through an RD Gateway or public bastion<a name="launch-wizard-iis-updates-bastion.title"></a>

To run Windows updates on the IIS servers by connecting from within a public resource such as an RD Gateway or bastion host:

1. Identify the public IP addresses for the public resource, and also the private IP addresses of the IIS servers, from the [Amazon EC2 Console](https://console.aws.amazon.com/ec2/)\.

1. Use the public IP of the public resource to [connect to the instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html)\.

1. From within the RDP connection to the public resource, use the IIS server's private IP addresses when creating subsequent RDP connections\.
**Note**  
You will use the nested RDP session within the public resource to the IIS server for the remaining steps\.

1. On the taskbar, open the **Start** menu, and choose **Settings**\.

1. In the Settings application, choose **Update & Security**

1. Choose **Check for updates**\.

1. Install any updates, and restart if necessary\.

## Testing the deployment<a name="launch-wizard-iis-testing"></a>

To test the deployment, ensure that your IP address being used is entered in the **WebAccessCIDR** parameter\. You can review previously entered **Parameters** for the Launch Wizard deployment in the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/)\. Next, you can use a web browser to navigate to the URL of the Application Load Balancer to confirm the test page is accessible\. The URL can be found in the **Outputs** of the AWS CloudFormation stack\.

![\[IIS post deployment test image.\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/iis-post-deploy-test.png)

## Connect to your Windows instances using SSM port forwarding sessions and RDP<a name="launch-wizard-iis-connect-ssm.title"></a>

You can connect to your deployed resources by completing some prerequisites for Amazon EC2 Systems Manager and using a [port forwarding session](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-sessions-start.html#sessions-start-port-forwarding) for the RDP connection\. The port forwarding method doesn't require bastion hosts, or for you to open inbound TCP 3389 connections to your resources\.

### SSM port forwarding prerequisites<a name="launch-wizard-iis-connect-ssm-prerequisites.title"></a>

To run Windows updates on the servers with Amazon EC2 Systems Manager and port forwarding for RDP sessions, the following prerequisites are required:
+ The [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) is installed on your computer opening the port forwarding session\.
+ The [AWS Command Line Interface \(AWS CLI\)](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html#cli-quick-configuration) is configured with security credentials for your AWS account\.
+ The [Session Manager plugin](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html#install-plugin-windows) for AWS CLI is installed\.
+ The SSM Agent is installed on your EC2 instances\.\*
+  An instance profile is attached which allows access to the Amazon EC2 Systems Manager API\.\*

\* These prerequisites are completed automatically as part of the Launch Wizard deployment\.

### Connect to your resources using SSM port forwarding sessions<a name="launch-wizard-iis-connect-ssm-steps.title"></a>

The following steps use the AWS CLI to start an SSM Session Manager connection on a specified Amazon EC2 instance and invoke the SSM document *AWS\-StartPortForwardingSession*\. This allows an RDP connection from your computer to the target Amazon EC2 instance using the redirected port\.

1. You can locate instance IDs to connect to from the [Amazon EC2 Console](https://console.aws.amazon.com/ec2/), for example *i\-1234567890abcdef0*\.

1. Run the following command in the AWS CLI by providing your target instance ID after the `--target` parameter, and a free local port on your computer for the `localPortNumber`

   ```
   aws ssm start-session --target "your-instance-id" --document-name AWS-StartPortForwardingSession --parameters "portNumber"=["3389"],"localPortNumber"=["56788"]
   ```  
![\[Establish a port forwarding session with the AWS CLI image.\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/iis-post-deploy-pf-cli-1.png)

1. When the session is established, open the Remote Desktop application, enter `localhost:56788`, and choose **Connect**\.  
![\[Connect with the RDP client using the port forwarding session image.\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/iis-post-deploy-pf-rdp-1.png)

1. Enter the credentials to the Amazon EC2 instance to log in\. You can find more information on retrieving the user name and password for your instance [here](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html)\.  
![\[Successful RDP connection over a port forwarding session image.\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/iis-post-deploy-pf-rdp-2.png)

1. When finished, you can exit the RDP session and end the AWS CLI session\.  
![\[End a port forwarding session with the AWS CLI image.\]](http://docs.aws.amazon.com/launchwizard/latest/userguide/images/iis-post-deploy-pf-cli-2.png)