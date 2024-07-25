## PROJECT

##### Context
We have been tasked by a company called Landmark Technology to build a CI CD ( Continuous Integration Continuous Deployment) pipeline for their new product launch. They want their customers to be able to register for a free demo with their first and last name as well as with their email address.

##### Tools
+ AWS
+ Jenkins CE
+ SCM (git/github)
+ Maven plugin
+ SonarQube
+ Nexus
+ Docker
+ DockerHub
+ KOPS
+ Kubernetes

##### Infrastructure
+ Cloud provider account: in our case AWS
+ 2 EC2 server t2.medium OS RHEL 7/8 and 1 EC2 server t2.medium OS ubuntu 24 - 04
+ Create security group and open port 22, 8080, 8081, 9000 which are the port we will be using for this project
+ Bind servers to that security group
+ 2 Auto scaling groups

![image](https://github.com/user-attachments/assets/39e3d776-4654-47f5-bb2b-bee2957eee81)
