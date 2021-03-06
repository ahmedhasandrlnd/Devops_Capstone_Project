# Devops Capstone Project
### Ahmed M. Hasan
### Udacity DevOps Nanodegree


## Introduction
In this project we'll apply the skills and knowledge which were developed throughout the Cloud DevOps Nanodegree program.
These include:
* Working in AWS
* Using Jenkins to implement Continuous Integration and Continuous Deployment
* Building pipelines
* Working with Ansible and CloudFormation to deploy clusters
* Building Kubernetes clusters
* Building Docker containers in pipelines

## Proposed Pipeline
![Pipeline](images/pipeline.JPG)

A Pipeline is a set of tools and processes to automate the CI/CD.

* Continuous Integration (CI) means newly developed code changes of a project are periodically built, tested, and integrated into a shared repository like Git. Then, the integrated code is verified and tested using automated tools.

* Continuous Delivery (CD) is the process of automating the release of the merged and validated code to a repository and finally release a production-ready build to the production environment.

## Deployment Strategies
Deployment strategy is an approach to roll out the updates/changes made in the "live" application. The idea is that the application must not be brought down to introduce the updates. There are a variety of strategies available. Let's assume that there are two versions of the software applications - version A and B. Version B is the updated version.

* Rolling - Version B is gradually rolled out succeeding version A. This is suitable when the updates are very small, such as bug fixes.
* Blue/Green - Version B is released alongside version A, then the traffic is switched to version B. This is the preferred model when there are major updates or releasing new features.
* Canary - Version B is released to a specific subset of users for early feedback and testing, then proceed to a complete rollout.
* A/B testing - Version B is released to a subset of users under particular conditions.
* Recreate - Version A is terminated first, and then the version B is rolled out.
* Shadow - Version B receives real-world traffic alongside version A and doesn’t impact the response.

We will follow Rolling deployment strategy in this project.

## Prerequisites
* AWS Account
* GitHub Account
* Docker ID

## Used Tools
* AWS EC2 instance
* Git
* Java
* Python
* Jenkins
* Docker
* AWS CLI
* Ansible
* AWS Elastic Kubernetes Service

## Set-up Docker Application
1. Launch an EC2 instance for Docker host

2. Install docker on EC2 instance and start services
```
yum install docker
service docker start
```
3. create a new user for Docker management and add him to Docker (default) group
```
useradd dockeradmin
passwd dockeradmin
usermod -aG docker dockeradmin
```
4. Install Hadolint
```
sudo wget -O /bin/hadolint https://github.com/hadolint/hadolint/releases/download/v1.16.3/hadolint-Linux-x86_64
sudo chmod +x /bin/hadolint
```

5. Write a Docker file under /opt/docker
```
mkdir /opt/docker
```
```
vi Dockerfile
# Pull base image 
From tomcat:8-jre8 

# Maintainer
MAINTAINER "Ahmed" 

# copy war file on to container 
COPY ./webapp.war /usr/local/tomcat/webapps
```

6. Login to Jenkins console and add Docker server to execute commands from Jenkins
Manage Jenkins --> Configure system --> Publish over SSH --> add Docker server and credentials

7. Create Jenkins job

A) Source Code Management
Repository : https://github.com/ahmedhasandrlnd/hello-world.git
Branches to build : */master

B) Build Root POM: pom.xml
Goals and options : clean install package

C) send files or execute commands over SSH Name: docker_host
Source files : webapp/target/*.war Remove prefix : webapp/target Remote directory : //opt//docker
Exec command[s] :
```
docker stop valaxy_demo;
docker rm -f valaxy_demo;
docker image rm -f valaxy_demo;
cd /opt/docker;
docker build -t valaxy_demo .
```

D) send files or execute commands over SSH
Name: docker_host
Exec command : 
```
docker run -d --name valaxy_demo -p 8090:8080 valaxy_demo
```

8. Login to Docker host and check images and containers. (no images and containers)

9. Execute Jenkins job

10. check images and containers again on Docker host. This time an image and container get creates through Jenkins job

11. Access web application from browser which is running on container
```
<docker_host_Public_IP>:8090
```


## Set-up Kubernetes Application
1. Create Ubuntu EC2 instance
2. install AWSCLI
 ```
 curl https://s3.amazonaws.com/aws-cli/awscli-bundle.zip -o awscli-bundle.zip
 apt install unzip python
 unzip awscli-bundle.zip
 #sudo apt-get install unzip - if you dont have unzip in your system
 ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
 ```
3. Install kubectl on ubuntu instance
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
 chmod +x ./kubectl
 sudo mv ./kubectl /usr/local/bin/kubectl
 ```
4. Install kops on ubuntu instance
```
 curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
 chmod +x kops-linux-amd64
 sudo mv kops-linux-amd64 /usr/local/bin/kops
 ```
5. Create an IAM user/role with Route53, EC2, IAM and S3 full access
6. Attach IAM role to ubuntu instance
7. Create a Route53 private hosted zone (you can create Public hosted zone if you have a domain)
```
Routeh53 --> hosted zones --> created hosted zone  
Domain Name: project.net
Type: Private hosted zone for Amzon VPC
```
8. create an S3 bucket
```
 aws s3 mb s3://devops.capstone.project.net
```
9. Expose environment variable:
``` 
export KOPS_STATE_STORE=s3://devops.capstone.project.net
```
10. Create sshkeys before creating cluster
```
ssh-keygen
```
11. Create kubernetes cluster definitions on S3 bucket
```
kops create cluster --cloud=aws --zones=us-west-2b --name=devops.capstone.project.net --dns-zone=project.net --dns private
``` 
12. Create kubernetes cluser
```
kops update cluster demo.k8s.capstone.net --yes
```
13. Validate your cluster
```
 kops validate cluster
```
14. To list nodes
```
kubectl get nodes
```
15. To delete cluster
``` 
kops delete cluster demo.k8s.capstone.net --yes
```

## Install AWSCLI
![Install AWSCLI](images/install_AWSCLI.JPG)

## Install Kubectl
![Insall kubectl](images/install_kubectl.JPG)

## Kubernetes Roles
![k8s role](images/k8s-role.JPG)

## Kubernetes clustes
![Kube Cluster](images/kube_cluster.JPG)

## Linting check- No Error
![No error](images/linting_test_noerror.JPG)

## Linting check- Error
![Error](images/linting_test_error.JPG)

## Blue Ocean
![Blue Ocean](images/blue_ocean.JPG)

## Version-A Deployment
![Version A](images/versionA_docker.JPG)

## Version-B Deployment (Rolling Deployment)
![Version B](images/versionB_docker.JPG)

## Route53 
![Route53](images/route53.JPG)

## EC2 Instances
![EC2_instance](images/ec2_instances.JPG)


## References
1. [Simple DevOps Project-1](https://www.youtube.com/watch?v=Z9G5stlXoyg)
1. [Simple DevOps Project-2](https://www.youtube.com/watch?v=nE4b9mW2ym0) 
1. [Simple DevOps Project 3](https://www.youtube.com/watch?v=nMLQgXf8tZ0)
1. [Simple DevOps Project 5](https://www.youtube.com/watch?v=gVjLDwcA6sQ)
