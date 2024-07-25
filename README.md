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

From the project dashboard in Jenkins go to Configuration - Build Trigger
![image](https://github.com/user-attachments/assets/71ad5fbe-9bc0-47e2-89eb-8344f8cd6a5d)

Go to the project repository on Github - Settings - Webhook

![image](https://github.com/user-attachments/assets/6855a9fe-2c98-4590-a436-24c0de13b076)

##### step 3 Jenkins - maven, SonarQube and Nexus configuration

From the project configuration page
![image](https://github.com/user-attachments/assets/7ab9e2ae-fcc3-4dc1-b3a6-9fa0a10c04cb)

##### step 4 Jenkins - Pipeline configuration

node
{
    
 stage('SCM Clone'){
     git 'https://github.com/Presick/spring-boot-docker'
  }   
    
  stage('maven build'){
      def mavenHome = tool name: 'maven3.6.3'
      def mavenCMD = "${mavenHome}/bin/mvn "
    sh "${mavenCMD} clean package"
    
  }  
    
 stage('QualityRreport'){
     sh '${mavenHome}/bin/mvn sonar:sonar'
  }
  
  stage('NexusUpload'){
     sh '${mavenHome}/bin/mvn deploy'
  }
  
  stage('BuildDockerImage'){
      
     sh "docker build -t ckeumo/spring-boot-mongo ."
  }
  
  
  stage('PushDockerImageToDockerhub'){
      
      withCredentials([string(credentialsId: 'hash', variable: 'hash')]) {
    sh "docker login registry-1.docker.io -u ckeumo -p ${hash} "
}
    
    sh "docker push ckeumo/spring-boot-mongo" 
       
  }
  
  stage('RemoveDockerImages'){
      
     sh "docker rmi ${docker images -q}"
  }
  
  stage('DeployAppIn K8S'){
      
     sh "kubectl apply -f springapp.yml"
  }
  
  
  
}


##### Deployment

![image](https://github.com/user-attachments/assets/dfdf12df-fd91-4eb1-b5b1-1a278c6ffc67) 
![image](https://github.com/user-attachments/assets/23c570f2-df50-4e34-ae03-354f54b94db3)
![image](https://github.com/user-attachments/assets/d26f3274-1508-4481-a56e-a718badddb64)
![image](https://github.com/user-attachments/assets/8dac0234-ade9-4749-bace-753c1ced1442)


## REFERENCE LINK
Project Github repository
https://github.com/Presick/spring-boot-docker
