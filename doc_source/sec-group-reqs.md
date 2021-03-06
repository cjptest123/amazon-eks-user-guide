# Cluster Security Group Considerations<a name="sec-group-reqs"></a>

If you create your VPC and worker node groups with the AWS CloudFormation templates provided in the [Getting Started with Amazon EKS](getting-started.md) walkthrough, then your control plane and worker node security groups are configured with our recommended settings\.

The security group for the worker nodes and the security group for the control plane communication to the worker nodes have been set up to prevent communication to privileged ports in the worker nodes\. If your applications require added inbound or outbound access from the control plane or worker nodes, you must add these rules to the security groups associated with your cluster\. For more information, see [Security Groups for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\.

**Note**  
To allow proxy functionality on privileged ports or to run the CNCF conformance tests yourself, you must edit the security groups for your control plane and the worker nodes\. The security group on the worker nodes side need to allow inbound access for ports 0\-65535 from the control plane, and the control plane side needs to allow outbound access to the worker nodes on ports 0\-65535\.

The following tables show the minimum required and recommended security group settings for the control plane and worker node security groups for your cluster:


**Control Plane Security Group**  

|  | Protocol | Port Range | Source | Destination | 
| --- | --- | --- | --- | --- | 
| Minimum inbound traffic |  TCP  |  443  |  Worker node security group  |  | 
| Recommended inbound traffic |  TCP  |  443  |  Worker node security group  |  | 
| Minimum outbound traffic |  TCP  |  10250  |  |  Worker node security group  | 
| Recommended outbound traffic |  TCP  |  1025\-65535  |  |  Worker node security group  | 


**Worker Node Security Group**  

|  | Protocol | Port Range | Source | Destination | 
| --- | --- | --- | --- | --- | 
| Minimum inbound traffic \(from other worker nodes\) |  Any protocol you expect your worker nodes to use for inter\-worker communication  |  Any ports you expect your worker nodes to use for inter\-worker communication  |  Worker node security group  |  | 
| Minimum inbound traffic \(from control plane\) |  TCP  |  10250  |  Control plane security group  |  | 
| Recommended inbound traffic |  All TCP  |  All 443, 1025\-65535  |  Worker node security group Control plane security group  |  | 
| Minimum outbound traffic\* |  TCP  |  443  |  |  Control plane security group  | 
| Recommended outbound traffic |  All  |  All  |  |  0\.0\.0\.0/0  | 

\* Worker nodes also require outbound internet access to the Amazon EKS APIs for cluster introspection and node registration at launch time\. To pull container images, they require access to the Amazon S3 and Amazon ECR APIs \(and any other container registries, such as DockerHub\)\. For more information, see [AWS IP Address Ranges](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) in the *AWS General Reference*\.