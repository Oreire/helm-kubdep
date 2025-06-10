# helm-kubdep

## Deployment of Containerized Web Applications on a Kubernetes Cluster Using Helm and GitHub Actions


## 1.0	Project Overview

This project implements the automated deployment of a containerized web application on a Kubernetes cluster using Helm charts. A three-node Kubernetes cluster was created using KIND (Kubernetes IN Docker) on Docker Desktop to simulate a production-like environment.
The deployment process involved building and containerizing the application using a Dockerfile, followed by pushing the image to a local container registry for efficient image management. Helm charts were then configured to define the application's Kubernetes resources, ensuring a standardized and repeatable deployment process.
To achieve continuous integration and deployment (CI/CD), GitHub Actions (self-hosted runner) was utilized to automate the Helm-based deployment pipeline. This ensured that any updates to the application were seamlessly built, tested, and deployed to the Kubernetes cluster. Finally, port-forwarding was used to enable access to the deployed application, allowing interaction with the running service within the cluster.
This project demonstrates a scalable, automated, and efficient approach to deploying containerized applications in a Kubernetes environment, leveraging Helm for package management and GitHub Actions for CI/CD automation.

## 2.0	Technologies Used

## Docker

Docker is a containerization platform that enables applications to be packaged into lightweight, portable containers. These containers encapsulate all dependencies, ensuring consistency across different environments. Docker allows services to run in isolated environments, either on the same host or distributed across multiple hosts, improving scalability and resource efficiency.

## Kubernetes

Kubernetes is an open-source container orchestration platform designed to deploy, manage, and scale containerized applications. It automates key processes such as service discovery, load balancing, and self-healing while ensuring high availability and fault tolerance. Kubernetes also supports auto-scaling, enabling applications to dynamically adjust resources based on demand.

## Helm

Helm is a package manager for Kubernetes that simplifies application deployment by bundling multiple Kubernetes resource files into a single package, known as a Helm chart. Helm allows applications to be installed with a single command, providing a default configuration while enabling customization through values files. This streamlines deployment, version control, and rollback processes, making Kubernetes application management more efficient.

## 3.0	Prerequisites

    •	Docker Desktop installed and running.
    •	Kubernetes enabled within Docker Desktop.
    •	Helm installed on your local machine.
    •	GitHub repository with the application source code.
    •	Self-hosted GitHub Actions runner configured on the local machine.
    •	Basic knowledge of Docker, Kubernetes, Helm, and GitHub Actions.


## 4.0	Step-by-Step Implementation

Follow this structured approach to implement CI/CD for deploying the containerized web application on a Kubernetes cluster running on Docker Desktop, using Helm and GitHub Actions. This workflow ensures continuous integration and deployment (CI/CD) by automating the build, testing, and deployment process using Kubernetes manifests, enabling a streamlined and repeatable deployment pipeline.

## Step 1: Containerize and Push Application 
    
    -	Create a Dockerfile for the web application: 
    
    -	Build the Docker image: docker build -t coweb .
    
    -	Push the image to Docker Hub: After tagging

## Step 2:  Create a Helm Chart

    -	Run the following command to generate a new Helm chart structure:
        helm create coweb

This creates a directory named coweb that contains default templates for deployments, services, and configurations.

## Step 3:  Define the Deployment
   
    -	Modify the Helm Deployment template (coweb/templates/deployment.yaml)
   
    -	Uses Helm template variables ({{ .Values.image.repository }}) to dynamically set the Docker image.
   
    -	Ensures two replicas of the web application are running.

## Step 4: Define the Service
    
    -	Modify the Helm Service template (coweb/templates/service.yaml)
    
    -	Exposes the web application on port 80 using Kubernetes NodePort.

## Step 5: Configure Helm Values
    
    -	Modify the default values in my-web-app/values.yaml
    
    -	This ensures Helm dynamically sets the Docker image repository and tag.

## Step 6: Deploy the Helm Chart
    
    -   Run the following command to install the Helm chart:
        
        helm install my-web-app ./my-web-app --namespace my-app
    
    -   Verify that the deployment and service are running:
        
        kubectl get pods -n coweb-ns
        kubectl get svc -n cpweb-ns

## Step 7: Automate CI/CD with GitHub Actions

    -   Configure the Self-Hosted Runner and start the runner:  .\rum.cmd or ./run.sh

## Step 8: Create GitHub Actions Workflow
    
    -	Create a .github/workflows/deploy.yml file in the repository.
    
    -	Define the workflow

## Step 9: Configure GitHub Secrets: if NOT using self-hosted runner

## Step 10: Trigger Deployment: Push changes to the repository: 

## Step 11: Access the Application
    
    -	Use port-forwarding to access the application: 
    
        kubectl port-forward svc/coweb-service 31999:80 -n coweb-ns
    
    -	Open the application in a browser: 
        
        http://localhost:31999





