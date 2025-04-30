# DevOps-Project-1
DevOps Project: Building, Deploying, and Monitoring a Full-Stack Application

This project was taken from prodevopsguy

# Introduction
DevOps is about automating processes, improving collaboration between development and operations teams, and deploying software more quickly and reliably. This project guides you through the creation of a comprehensive CI/CD pipeline using industry-standard tools. You will deploy a full-stack application on AWS using Jenkins, Docker, Kubernetes (Amazon EKS), Prometheus, Grafana, Trivy, SonarQube, and Terraform. This hands-on experience will help you master key DevOps concepts and tools.

# Pre-requizite 
1. Create a github repo
2. Clone that onto yout local machine
3. Down load and install the aws cli 
4. allow auto-promt on the aws cli

# Project Diagram
  +------------------------+
  |   Developer Workstation |
  |                        |
  |  - Code Repository     |
  |  - Local Build & Test  |
  +-----------+------------+
              |
              v
  +------------------------+
  |        Jenkins         |
  |                        |
  |  - CI/CD Pipeline      |
  |  - Build & Test        |
  |  - Docker Build        |
  |  - Push Docker Image   |
  +-----------+------------+
              |
              v
  +------------------------+          +----------------------+
  |        Docker Hub      |          |        AWS EKS        |
  |                        |          |                      |
  |  - Docker Image        |          |  - Kubernetes Cluster |
  |                        |          |                      |
  +-----------+------------+          +-----------+----------+
              |                                    |
              v                                    |
  +------------------------+          +----------------------+
  |   Kubernetes Deployment|          |  Prometheus & Grafana|
  |                        |          |                      |
  |  - Deployment          |          |  - Monitoring         |
  |  - Service             |          |  - Dashboards        |
  |                        |          |                      |
  +------------------------+          +----------------------+
              |
              v
  +------------------------+
  |     Amazon RDS         |
  |                        |
  |  - MySQL Database      |
  |                        |
  +------------------------+

# Project Overview

Project Overview
----------------

### Objectives

   Infrastructure Setup: Provision AWS resources including VPC, EC2 instances, and RDS databases.
    
   CI/CD Pipeline: Automate the build, test, and deployment processes with Jenkins.
    
   Containerization: Containerize the application using Docker.
    
   Kubernetes Deployment: Deploy the application on Amazon EKS.
    
   Monitoring: Implement continuous monitoring using Prometheus and Grafana.
    
   Security: Secure the pipeline with Trivy and SonarQube.
    
   Infrastructure as Code: Automate infrastructure management with Terraform.
    
   Blue-Green Deployment: Implement blue-green deployment strategies.
    

### Tools and Technologies

   AWS: EC2, VPC, RDS, EKS.
    
   Jenkins: CI/CD automation.
    
   Docker: Containerization.
    
   Kubernetes: Container orchestration.
    
   Prometheus & Grafana: Monitoring and visualization.
    
   Trivy & SonarQube: Security and code quality checks.
    
   Terraform: Infrastructure as code.


   # Step 1: Infrastructure Setup on AWS
   ## 1.1 Create vpc
```bash
aws ec2 create-vpc --cidr-block 10.0.0.0/16
```

Configure subnets:
```bash
aws ec2 create-subnet --vpc-id <vpc-id> --cidr-block 10.0.1.0/24 --availability-zone us-east-1a

```

   Set up internet Gateway:
```bash
aws ec2 create-internet-gateway
 aws ec2 attach-internet-gateway --vpc-id <vpc-id> --internet-gateway-id <igw-id>
```

create route tables and associated with subnet
```bash
aws ec2 create-route-table --vpc-id <vpc-id>
aws ec2 create-route --route-table-id <rtb-id> --destination-cidr-block 0.0.0.0/0 --gateway-id <igw-id>
aws ec2 associate-route-table --subnet-id <subnet-id> --route-table-id <rtb-id>
aws ec2 modify-subnet-attribute \
  --subnet-id <subnet-id> \
  --map-public-ip-on-launch
```

### Set up security group
create a security group
```bash
   aws ec2 create-security-group --group-name MySecurityGroup --description "Security group for my app" --vpc-id <vpc-id>
```

Allow SSh, HTTP, and HTTPS
```bash
aws ec2 authorize-security-group-ingress --group-id <sg-id> --protocol tcp --port 22 --cidr 0.0.0.0/0
aws ec2 authorize-security-group-ingress --group-id <sg-id> --protocol tcp --port 80 --cidr 0.0.0.0/0
aws ec2 authorize-security-group-ingress --group-id <sg-id> --protocol tcp --port 443 --cidr 0.0.0.0/0
```
## 1.2 Provissioning EC2 Instance

Launch EC2 instance
```bash
 aws ec2 run-instances --image-id ami-0abcdef1234567890 --count 1 --instance-type t2.micro --key-name MyKeyPair --security-group-ids <sg-id> --subnet-id <subnet-id>
```

Install Docker and Jenkins on the EC2 instance:
```bash
 sudo yum update -y
 sudo yum install docker -y
 sudo service docker start
 sudo usermod -a -G docker ec2-user

 # Jenkins
 sudo yum install java-1.8.0-openjdk -y
 wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
 rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
 sudo yum install jenkins -y
 sudo systemctl start jenkins
 sudo systemctl enable jenkins
```

## Setting up an RDS databse

### Provision an RDS Instance:
create a MSQL instance
```bash

```