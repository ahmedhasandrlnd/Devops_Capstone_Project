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
