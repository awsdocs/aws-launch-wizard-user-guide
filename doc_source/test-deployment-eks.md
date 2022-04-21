# Test the deployment<a name="test-deployment-eks"></a>

**Important**  
You must run these steps from a network that has access to the Kubernetes API, as configured by the **Amazon EKS public access endpoint** and **Kubernetes API public access CIDR** parameters\. For more information, see [Installing kubectl](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html) in the **Amazon EKS User Guide**\. If you enabled the optional bastion host, you can connect to it by using SSH\. Use the key pair that you specified during deployment and the IP address from the **Outputs** tab of the AWS CloudFormation stack\. The bastion host already has `kubectl` installed and configured so that it connects to the cluster\. To test the CLI, connect to the cluster, and run the command, shown in step 1\.

**Follow this procedure to test the deployment:**

1. Run the following command:

   ```
   $ kubectl version
   ```

1. Confirm that the output includes the server version, which indicates a successful connection to the Kubernetes control plane\.

   ```
   Client Version: version.Info\{Major:"1", Minor:"11", GitVersion:"<version number>", GitCommit:"<commit ID>", 
   GitTreeState:"clean", BuildDate:"2018-12-06T01:33:57Z", GoVersion:"go1.10.3", Compiler:"gc", Platform:"linux/amd64"}
   
   Server Version: version.Info\{Major:"1", Minor:"11+", GitVersion:" <version number>", GitCommit:" <commit ID>", 
   GitTreeState:"clean", BuildDate:"2018-12-06T23:13:14Z", GoVersion:"go1.10.3", Compiler:"gc", Platform:"linux/amd64"}
   ```

1. Check for a successful connection between the nodes and cluster by running the `kubectl get nodes` command\.

   ```
   $ kubectl get nodes
   NAME STATUS ROLES AGE VERSION
   ip-10-0-25-239.us-west-2.compute.internal Ready <none> 10m <version number>
   ip-10-0-27-244.us-west-2.compute.internal Ready <none> 10m <version number>
   ip-10-0-35-29.us-west-2.compute.internal Ready <none> 10m <version number>
   ```