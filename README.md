## Disclaimer
This is not an Officially Supported Google Product!



## How to Setup Apigee hybrid on AWS EKS Clusters using Terraform 


The terraform configuration defines a new VPC in which to provision the cluster, and uses the public EKS module to create the required resources, including Auto Scaling Groups, security groups, and IAM Roles and Policies.
Open the main.tf file to review the module configuration. The eks_managed_node_groups parameter configures the cluster with three nodes across two node groups.


## Pre-Cluster Setup Steps
1. Perform part 1 of the Apigee Hybrid setup to create your APigee organization, environment and environment group
2. Run aws configure
  2a. Customise the main.tf with your worker nodes and labels
  2b. Customise terraform.tf

3. Run Terraform apply and eks cluster comes up 

##  Accessing the Cluster
Once the cluster is up run the below command to gain access to the cluster 

```
aws eks --region $(terraform output -raw region) update-kubeconfig \
    --name $(terraform output -raw cluster_name)
```

Now you can run 
```
kubectl get pods -A
```
and have an output similar to the image below

## Apigee Hybrid Installation steps
Now proceed with part 2 of the setup steps to install Apigee hybrid




### Multiple clusters

To create multiple clusters perform the following steps

clone the repo to another folder (or copy existing and delete terraform state files and folder)

edit the main.tf file by updating the vpc name, cluster name 
edit the variables.tf by updating the aws region



### Clean Up

To perform a clean up of the aws resources created by terraform 
Step 1. Delete the aws loadbalancers created ( these get created when the ingress is created and also another one when the kubernetes service in part 3 is created). Alternative to this stepw ould be to import the loadbalancers created manually with terraform import so terraform can manage the destruction of these going forward.

Step 2. Run terraform destroy to delete the aws resources created by terraform 
