# Azure Day-29 – Azure Container Registry (ACR) Setup & Image Push

The Nautilus DevOps team needs to:
Create an Azure Container Registry (ACR)
Build a Docker image from an existing Dockerfile
Push the image to the ACR repository
This allows container images to be securely stored, versioned, and deployed.

cd /root/pyapp ; ls
# Navigate to the application directory and list files to confirm Dockerfile exists

cat app.py
# View the Python application code that will be packaged inside the Docker image

az acr create --name datacenteracr25745 --resource-group <RESOURCE_GROUP_NAME> --location eastus --sku Basic
# Create Azure Container Registry named datacenteracr25745 in East US with Basic pricing plan

az acr login --name datacenteracr25745
# Log in to Azure Container Registry to allow Docker image push operations

docker build -t datacenteracr25745.azurecr.io/datacenteracr25745:latest .
# Build Docker image from Dockerfile in current directory and tag it as latest for ACR

docker push datacenteracr25745.azurecr.io/datacenteracr25745:latest
# Push the Docker image to Azure Container Registry repository

✅ Final Outcome
ACR datacenteracr25745 created in East US
Pricing plan set to Basic
Docker image built from /root/pyapp

Image successfully pushed as latest to Azure Container Registry
