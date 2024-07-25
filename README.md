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

##### WORFLOW

##### description
+ Once the developer commit the code and once this one is merged to the main branch, a build is triggered on Jenkins via webhook
+ The artefact is successively built( Maven), analyzed for quality ( SonarQube) then a copy of the .war file is stored in Nexus. Next the artefact is containerize and a copy is pushed to DockerHub where it is pulled and deployed in the Kubernetes cluster previously built AWS using KOPS.
![image](https://github.com/user-attachments/assets/5d285731-584c-4d65-95f1-138528bd9b26)

##### step 1 create the project on Jenkins dashboard

On Jenkins dashboard: + NewItem - Name the project ( spring-java-app-mongo) - Select Freestyle Project - click OK
![image](https://github.com/user-attachments/assets/8e363deb-1f3d-496e-b98e-ab07f1b328e4)

##### step 2 Jenkins - Git integration + Webhook configuration

From the project dashboard in Jenkins go to Configuration - Source Code Management


