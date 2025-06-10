# istio-kubdep
Automated Deployment of Containerised Web Applications on K8S Cluster Using Helm and GitHub Actions 


helm installed into /usr/local/bin/helm
ajayi@GLANIK:/mnt/c/ISTIO/istio-kubdep$ helm version
version.BuildInfo{Version:"v3.18.2", GitCommit:"04cad4610054e5d546aa5c5d9c1b1d5cf68ec1f8", GitTreeState:"clean", GoVersion:"go1.24.3"}
ajayi@GLANIK:/mnt/c/ISTIO/istio-kubdep$

deploy.yaml

name: Helm Deployment (Local)

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: self-hosted
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Build Local Docker image
        run: |
          cd WEB
          docker build --no-cache -t coweb-app:latest .
          docker tag coweb-app:latest localhost:5000/coweb-app:latest
          docker push localhost:5000/coweb-app:latest

      - name: Ensure coweb-ns namespace exists
        run: |
          kubectl get namespace coweb-ns || kubectl create namespace coweb-ns

      - name: Install kubectl
        uses: azure/setup-kubectl@v3
        with:
          version: 'latest'

      - name: Set Kubernetes Context
        run: |
          kubectl config use-context docker-desktop

      - name: Apply Kubernetes Manifests
        run: |
          kubectl apply -f DEPLOY/

      - name: Install Helm and Deploy Application
        run: |
          helm upgrade --install coweb ./coweb `
            --namespace coweb-ns `
            --set image.repository=coweb `
            --set image.tag=latest `
            --set image.pullPolicy=Always

      - name: Lint Helm Chart
        run: |
          helm lint ./coweb

      - name: Force Deployment to Use Latest Image
        run: |
          kubectl rollout restart deployment/coweb -n coweb-ns

      - name: Verify Deployment Status
        run: |
          kubectl get all -n coweb-ns

