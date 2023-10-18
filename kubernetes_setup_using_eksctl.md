# Setup Kubernetes on Amazon EKS

You can follow same procedure in the official  AWS document [Getting started with Amazon EKS – eksctl](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)   

#### Pre-requisites: 
  - an EC2 Instance 

#### AWS EKS Setup 
1. Setup kubectl   
   a. Download kubectl version 1.20  
   b. Grant execution permissions to kubectl executable   
   c. Move kubectl onto /usr/local/bin   
   d. Test that your kubectl installation was successful    
   ```sh 
   curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
   chmod +x ./kubectl or chmod +x kubectl
   mv ./kubectl /usr/local/bin 
   kubectl version --short --client
   ```
2. Setup eksctl   
   a. Download and extract the latest release   
   b. Move the extracted binary to /usr/local/bin   
   c. Test that your eksclt installation was successful   
   ```sh
   curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
   sudo mv /tmp/eksctl /usr/local/bin
   eksctl version

   ll /usr/local/bin  ->check kubectla and eksctl
   ```
  
3. Create an IAM Role and attache it to EC2 instance    
   `Note: create IAM user with programmatic access if your bootstrap system is outside of AWS`
   create IAM Role-> IAM->ROle->select Common Usecase as EC2
     
   IAM user should have access to   
   IAM   --IAMFullAccess
   EC2   --AmazonEc2FullAceess
   VPC    --AmazonVPCFullAcess
   CloudFormation  ---AWSCloudFormationFullAcess

   and also AdministiveAcess

   Then add this role to EC2 ->select Ec2 instence->Manage-security->Modify IAM-Select new created Bootstrap Role

5. Create your cluster and nodes 
   ```sh
   eksctl create cluster --name cluster-name  \
   --region region-name \
   --node-type instance-type \
   --nodes-min 2 \
   --nodes-max 2 \ 
   --zones <AZ-1>,<AZ-2>
   
   example:
   eksctl create cluster --name valaxy-cluster \
      --region ap-south-1 \
   --node-type t2.small \
    ```

   --Once executed this this CloudFormation stack,EC2 instace,EKS cluster get created
   you can see worker nodes in EC2 instce but master node will be managed by aws

7. To delete the EKS clsuter 
   ```sh 
   eksctl delete cluster valaxy --region ap-south-1
   ```
   
8. Validate your cluster using by creating by checking nodes and by creating a pod 
   ```sh 
   kubectl get nodes
   kubectl run pod tomcat --image=tomcat

   eksctl get cluster
   ```

