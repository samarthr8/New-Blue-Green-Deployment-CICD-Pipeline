Banking Transaction Application

This repository contains the source code for a Banking Transaction Application built using Java (Maven), utilizing MySQL for data storage. The repository is designed for a complete CI/CD workflow, using tools like Jenkins, SonarQube, Trivy, Nexus, Docker, and Kubernetes. The application can be deployed on a Kubernetes cluster.
Project Overview

The application is a banking transaction system where users can perform basic banking operations. It is containerized using Docker and can be deployed using Kubernetes. The Jenkins pipeline provided automates the entire build, test, scan, and deployment processes.
Repository Structure

    Source Code: Java-based application for managing banking transactions, built using Maven.
    Dockerfile: Defines the Docker image for the application.
    Jenkinsfile: Automates CI/CD pipeline tasks.
    Kubernetes Manifests: Configuration files for deploying the application on a Kubernetes cluster.

Prerequisites

    Java: Ensure you have Java installed on your local machine.
    Maven: Required for building and managing the Java application.
    Docker: Required for containerizing the application.
    Jenkins: For managing the CI/CD pipeline.
    Kubernetes: For deploying the application.
    MySQL: Database for storing the application's data.

Jenkins Pipeline Overview

The Jenkinsfile automates the following tasks:

    Clean workspace.
    Checkout source code from the Git repository.
    Compile the code using Maven.
    Run unit tests to ensure code quality.
    Scan code using SonarQube.
    Scan the application using Trivy for vulnerabilities.
    Build the application using Maven.
    Push artifacts to Nexus for storage.
    Build the Docker image and tag it as either blue or green.
    Scan Docker image using Trivy.
    Push Docker image to DockerHub.
    Deploy the application to the Kubernetes cluster.

Setting up the Environment
EC2 Setup

    Create an EC2 t2.medium instance with 20 GB storage and the following ports open in the security group: 22, 443, 80, 500-1000, 1000-11000.
    Name the instance Server.
    Run the following commands:

    bash

    sudo apt update
    sudo apt install awscli
    sudo snap install terraform --classic

    Attach the AdministratorAccess role to the EC2 instance.

Terraform Deployment

    Use the Terraform-installed EC2 instance (Server) to provision:
        VPC
        Subnets
        Security Groups
        Internet Gateway
        Route Table
        4 EC2 instances for:
            Kubernetes
            Jenkins
            SonarQube
            Nexus

    All instances should be t2.medium with 20 GB storage.

    SSH into each EC2 instance and set hostnames as per the roles:
        Kubernetes
        Jenkins
        SonarQube
        Nexus

Kubernetes Configuration

    On the Kubernetes EC2 instance:
        Install kubectl, eksctl, and awscli.
        Attach the AdministratorAccess role to the instance.
        Create a Kubernetes cluster with at least 3 nodes using a cluster.yaml file with eksctl.

Jenkins Configuration

    On the Jenkins EC2 instance:
        Install Java, Jenkins, Docker, kubectl, and awscli.
        Add the Jenkins user to the Docker group.

    Configure the Jenkins plugins:
        sonar-scanner
        docker-pipeline
        maven-pipeline
        kubernetes
        nexus

    Restart the Jenkins server and configure tools like Maven 3 and SonarQube in Jenkins.

    Run the following commands in the Jenkins terminal to configure access to the Kubernetes cluster:

    bash

    kubectl create serviceaccount jenkins
    kubectl create role jenkins-role --verb=* --resource=*
    kubectl create rolebinding jenkins-rolebind --role=jenkins-role --serviceaccount=default:jenkins
    kubectl create token jenkins-token

    Add credentials in Jenkins for:
        Docker Hub (docker-cred)
        SonarQube token (sonar-token)
        Kubernetes token

SonarQube and Nexus Configuration

    On both SonarQube and Nexus EC2 instances:
        Install Docker and run the SonarQube and Nexus as containers.

    Configure the web interfaces of SonarQube and Nexus for login and access.

Apply Jenkinsfile

    Apply the Jenkins pipeline configuration and monitor the deployment of the application on the Kubernetes cluster.

Usage

    Clone the repository:

    bash

    git clone https://github.com/your-repo/banking-transaction-app.git

    Follow the Jenkins pipeline setup to build and deploy the application.

    Access the application via the Kubernetes cluster's external endpoint.

Tools Used

    Java
    Maven
    Docker
    Jenkins
    SonarQube
    Trivy
    Nexus
    Kubernetes
    Terraform
    MySQL
