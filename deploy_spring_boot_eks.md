# Deploy Spring-boot image to EKS Cluster

  - Login to EC2 Instace from cluster is created (Main Ec2 instance [Manually Created])
  - install Jenkins to main ec2 inatance and follows k8s deployment

  - in Spring Boot apps add Docker file

     ![image](https://github.com/rtls-net/eks_k8s_setup/assets/147226341/d628d8e4-9454-42a6-aa61-b676b6af5536)

  - then build spring boot apps
  
       ./mvn clean package

  - build Docker image

     Docker build -t <image-name> .

- Test Docker image 

   Docker run -p 8080:8080 -it --rm --name my-running-app <image-name>
   check in another terminal

  curl localhost:8080

- Go to ECR and create a repo
- follow below screen command to build and push image to ECR 

 ![image](https://github.com/rtls-net/eks_k8s_setup/assets/147226341/f7038d2b-cf94-4d13-a11c-0c30b92abf1d)


- Now create EKS Cluster or use eks created cluster using eksctl and kubectl document in this repo.

- create deployemnt and service object in spring boot  deployemt.yaml adn srvc.yaml make sure you have change the image name copied from ECR image->copy URI.

![image](https://github.com/rtls-net/eks_k8s_setup/assets/147226341/318968d1-c9a1-4f4b-9191-edb3a9174599)

![image](https://github.com/rtls-net/eks_k8s_setup/assets/147226341/3dc269c4-5309-4e6f-8e45-6cc49e127a8f)

- apply deployment.yaml and srvc.yaml from main EC2 console assume both in k8s.yaml

   kubectl apply -f k8s.yaml

  kubectl get pods
  kubectl get nodes
  kubectl get svc

 - view  jar in eks system
   
   kubectl exec -it <pod-name> -- bash

   o/p -  <myapp.jar>
   exit from integrated terminal

-kubectl get svc
   copy external ip and check able to ping by using nslookup <external-ip> -> access apps-> curl <external-ip>
#Deployemnt Done !

=============================================================
###  if svc is LoadBalance 
 Need ingress setup
    -
    
  








