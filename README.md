 DevOps Tasks & Instructions — CodeAlpha

TASK 1: CI/CD Pipeline using Azure 
● Build an automated CI/CD pipeline with Azure Pipelines. 
● Use Azure Container Registry for container storage. 
● Deploy web apps via Azure App Service automatically.
 ● Monitor pipeline to ensure smooth execution . 
● Learn key DevOps concepts using Azure tools

✅ Objective
Create an automated CI/CD pipeline using Azure Pipelines to:

Build a containerized web app.

Push the image to Azure Container Registry (ACR).

Deploy the container to Azure App Service (for containers).

Enable monitoring to ensure reliability.

Learn core DevOps practices.

🛠️ Tools/Services Required
Azure DevOps

Azure Pipelines

Azure Container Registry (ACR)

Azure App Service (Linux, for Containers)

Monitoring with Azure Application Insights (optional but recommended)

📦 Step 1: Prepare Your Web App (Example: Node.js or Python)
Create a sample web application. Example: app.js or app.py

Add a Dockerfile:

dockerfile
Copy
Edit
# For a Node.js app
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["node", "app.js"]
🏗️ Step 2: Set up Azure Resources
A. Create a Resource Group
bash
Copy
Edit
az group create --name DevOpsRG --location eastus
B. Create an Azure Container Registry (ACR)
bash
Copy
Edit
az acr create --resource-group DevOpsRG --name MyUniqueACRName --sku Basic
C. Create an App Service Plan & Web App for Containers
bash
Copy
Edit
az appservice plan create --name MyAppServicePlan --resource-group DevOpsRG --is-linux --sku B1

az webapp create --resource-group DevOpsRG --plan MyAppServicePlan \
  --name mywebapp-unique-name --deployment-container-image-name myacr.azurecr.io/myapp:latest
🧪 Step 3: Set up Azure DevOps CI/CD Pipeline
A. Create a new Azure DevOps project
B. Create a Service Connection to Azure
Go to Project Settings > Service connections

Add a Docker Registry or Azure Resource Manager connection

C. Add your code repository (GitHub or Azure Repos)
🔧 Step 4: Create azure-pipelines.yml in the root of the repo
yaml
Copy
Edit
trigger:
  branches:
    include:
      - main

variables:
  imageName: 'myapp'

stages:
- stage: Build
  jobs:
  - job: Build
    pool:
      vmImage: 'ubuntu-latest'

    steps:
    - task: Docker@2
      inputs:
        containerRegistry: 'MyACRServiceConnection'
        repository: '$(imageName)'
        command: 'buildAndPush'
        Dockerfile: '**/Dockerfile'
        tags: 'latest'

- stage: Deploy
  dependsOn: Build
  jobs:
  - job: Deploy
    pool:
      vmImage: 'ubuntu-latest'
    steps:
    - task: AzureWebAppContainer@1
      inputs:
        azureSubscription: 'MyAzureServiceConnection'
        appName: 'mywebapp-unique-name'
        containers: 'myacr.azurecr.io/myapp:latest'
🔍 Step 5: Monitoring
Option A: Use Azure Application Insights
Enable it in the App Service → Settings > Application Insights > Enable

Option B: Azure Monitor
Use Log Analytics to monitor logs from your App Service and pipelines.

📘 DevOps Concepts 
Concept	                          How It’s Used
CI (Continuous Integration)	 Code pushed to repo triggers build
CD (Continuous Deployment)	 Auto deploy to App Service post-build
Infrastructure as Code	         Pipeline YAML defines CI/CD config
Containerization	         Docker used to package the app
DevOps Monitoring	         Azure App Insights / Monitor for observability

Summary: 
 App Dockerized

 ACR configured

 Web App created with container support

 Azure DevOps pipeline working

 Build → Push → Deploy flow automated

 Monitoring enabled



TASK 4: Web Server using Docker 
● Learn Docker containerization basics. 
● Deploy and manage a web server inside Docker containers. 
● Understand container lifecycle and commands.
 ● Monitor container health and troubleshoot issues. 
● Explore container-based app deployment best practice


Objective
Learn Docker basics and:

Deploy a web server (e.g., Nginx, Apache, or custom app) in Docker.

Understand container lifecycle and how to manage them.

Monitor and troubleshoot container issues.

Learn best practices for container-based deployment.

🧠 1. Docker Basics — Key Concepts
Concept	Description
Image	A snapshot of a filesystem with code and dependencies.
Container	A running instance of an image.
Dockerfile	A script that defines how to build an image.
Docker Hub	Public registry to store and pull images.
Volume	Used for data persistence.
Port Mapping	Maps container ports to host ports.
Healthcheck	Monitors container health.

🧪 2. Learn by Doing — Deploy a Web Server in Docker
🔸 Option A: Use a Prebuilt Image (e.g., Nginx)
bash
Copy
Edit
docker pull nginx
docker run -d -p 8080:80 --name web-server nginx
-d: Run in background

-p 8080:80: Map host port 8080 to container port 80

nginx: Image name

Visit: http://localhost:8080 🎉

🔸 Option B: Create Your Own Web Server Image
Example: Custom HTML with Nginx
Directory structure:

pgsql
Copy
Edit
webserver/
├── Dockerfile
└── index.html
Dockerfile:

Dockerfile
Copy
Edit
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
index.html:

html
Copy
Edit
<!DOCTYPE html>
<html>
<head><title>Hello from Docker!</title></head>
<body><h1>Hello World 🎉</h1></body>
</html>
Build and Run:

bash
Copy
Edit
docker build -t custom-nginx .
docker run -d -p 8080:80 --name my-nginx custom-nginx
🔁 3. Understand the Container Lifecycle
Common Commands:
bash
Copy
Edit
docker ps -a               # List containers
docker start <name/id>     # Start stopped container
docker stop <name/id>      # Stop container
docker rm <name/id>        # Remove container
docker logs <name/id>      # View container logs
docker exec -it <name> sh  # Run shell inside container
❤️‍🩹 4. Monitor Container Health
Add a Health Check in Dockerfile:
Dockerfile
Copy
Edit
HEALTHCHECK CMD curl --fail http://localhost || exit 1
Monitor:
bash
Copy
Edit
docker inspect --format='{{json .State.Health}}' <container_name>
🛠️ 5. Troubleshooting Tips
Symptom	What to Check
Container won't start	docker logs <name>
Site not loading	Check port mapping (-p)
File not found	Check Dockerfile COPY path
Container exits	Check CMD/ENTRYPOINT in Dockerfile

🧠 6. Best Practices for Container-Based Deployment
✅ Keep Dockerfiles minimal
✅ Use .dockerignore to avoid copying unnecessary files
✅ Use multi-stage builds for production apps
✅ Tag your images (e.g., v1.0)
✅ Clean up unused containers/images:


✅ Task Summary Checklist

 Docker installed and understood

 Web server deployed in Docker (Nginx or custom)

 Docker container lifecycle understood and practiced

 Health checks and logs monitored
