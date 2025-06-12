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
        
        helm install coweb ./coweb --namespace my-app
    
    -   Verify that the deployment and service are running:
        
        kubectl get pods -n coweb-ns
        kubectl get svc -n coweb-ns

## Step 7: Automate CI/CD with GitHub Actions

    -   Configure the Self-Hosted Runner and start the runner:  .\rum.cmd or ./run.sh

## Step 8: Create GitHub Actions Workflow
    
    -	Create a .github/workflows/deploy.yml file in the repository.
    
    -	Define the workflow

## Step 9: Configure GitHub Secrets: if NOT using self-hosted runner

## Step 10: Trigger Deployment: Push changes to the repository: 

## Step 11: Access the Application
    
    -	Use port-forwarding to access the application: 
    
        kubectl port-forward svc/coweb-service 32500:80 -n coweb-ns
    
    -	Open the application in a browser: 
        
        http://localhost:32500

## 5.0 Key Project Highlights

## Helm Linting Workflow

The helm lint ./coweb command validates the Helm chart in the coweb directory, checking Chart.yaml, values.yaml, and templates for syntax errors, missing values, and adherence to best practices. Helm processes templates to ensure correct references to defined values. If misconfigurations such as undefined .Values references, incorrect field types, or structural issues are detected, it generates warnings or errors that may lead to deployment failures. Linting helps maintain Helm charts by ensuring consistency across environments and preventing runtime errors. As a key step in CI/CD pipelines, it catches issues before deployment, reducing risks and improving reliability.

## CodeRabbit

CodeRabbit is an AI-powered code review tool that integrates with GitHub to automate pull request reviews. Installed in VS Code and triggered on push, it analyzes code changes, summarizes identified issues, and suggests improvements using AI models. It can also be integrated into CI/CD pipelines by authorizing CodeRabbit on GitHub, configuring it for the repository, and enabling it in GitHub Actions. This provides several benefits, including reducing manual code reviews, AI-powered issue detection, continuous feedback on commits within pull requests, and customizable rules via .coderabbit.yaml.

## Helm Rollbacks & Kubernetes Health Checks

Helm rollbacks ensure application stability by reverting deployments to previous functional versions in case of failures. Users can restore an earlier release by specifying the release name and revision. Tracking rollback history improves transparency, helping teams understand deployment evolution.

Kubernetes health checks enhance application reliability through liveness and readiness probes. Liveness probes restart unhealthy containers, while readiness probes prevent unready instances from receiving traffic. Configuring these probes in deployment.yaml improves uptime and mitigates cascading failures.

Integrating Helm rollbacks with Kubernetes health checks strengthens deployment resilience, minimizing downtime and enhancing fault tolerance. Automating rollback triggers based on failed health checks ensures rapid recovery. Teams can further optimize workflows by incorporating Kubernetes StatefulSets for consistent recovery strategies.

## CI/CD Integration with Helm As Manager

Integrating Helm chart deployment with CI/CD automation using GitHub Actions streamlines updates. Changes pushed to a repository trigger automatic Helm deployments while validating configurations through linting to catch errors early. In case of deployment failures, Helm’s rollback mechanism restores the previous stable version, preventing disruptions. Additionally, Helm's upgrade strategy enables zero-downtime updates, maintaining application availability.


**Overall**, by combining Helm rollbacks, Kubernetes health checks, CI/CD automation, and AI-powered code reviews, deployments become more resilient and self-healing. This reduces manual intervention while ensuring fast recovery from failures, making it ideal for production environments where uptime and stability are critical. 

## 6.0	Next-Steps

To optimize this CI/CD workflow for production environments, consider the following enhancements:
   
    -	Enhanced Security and Secrets Management via Role-Based Access Control (RBAC) to ensure secure Helm deployments.
   
    -	GitOps integration for further enhanced deployment reliability using ArgoCD
   
    -	Automated Unit, Integration and Smoke Testing before deployment to validate the containerized application.
   
    -	Improved Observability using Istio






