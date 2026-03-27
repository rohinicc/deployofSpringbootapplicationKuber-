🚀 Spring Boot & MySQL Deployment on Kubernetes (Kubeadm)

This repository contains a production-style deployment of a Spring Boot application with a MySQL database on a Kubernetes cluster provisioned using kubeadm.

It demonstrates a complete DevOps workflow including containerization, orchestration, networking, persistent storage, and deployment automation on AWS EC2.

🏗️ Architecture Overview

The system follows a multi-tier, decoupled architecture:

Application Layer: Spring Boot (Java) deployed using Kubernetes Deployment
Database Layer: MySQL deployed using StatefulSet (ensures stable identity & storage)
Networking Strategy:
ClusterIP Service: Internal communication between app and database
NodePort Service: External access via EC2 Public IP
Storage: Persistent Volume (PV) and PVC using local-path-provisioner
🔄 Architecture Flow
User (Browser)
      │
      ▼
[ EC2 Public IP : NodePort ]   (NodePort Service)
      │
      ▼
[ Spring Boot Pod ]            (Deployment)
      │
      ▼
[ MySQL Service ]              (ClusterIP - Internal)
      │
      ▼
[ MySQL Pod ]                  (StatefulSet)
      │
      ▼
[ Persistent Volume ]          (Storage)
🧰 Technology Stack
Backend: Spring Boot (Java)
Database: MySQL
Containerization: Docker
Orchestration: Kubernetes (kubeadm)
Infrastructure: AWS EC2 (Ubuntu 22.04)
Scripting: Bash
📁 Project Structure
spring_mysql_kubeadm_sakcoorg/
│
├── kube_scripts/
│   ├── app-deploy-svc.yml        # Spring Boot Deployment + NodePort Service
│   ├── db-statefullset-svc.yml   # MySQL StatefulSet + ClusterIP Service
│   └── setup-storage.sh          # Storage setup script
│
├── Dockerfile                    # Multi-stage Docker build
├── k8s-deploy.sh                # Interactive deployment script
├── docker_build_push.sh         # Docker build & push automation
└── src/                         # Application source code
⚙️ Deployment Workflow
1️⃣ Build & Push Docker Image

Ensure Docker is logged in, then run:

bash docker_build_push.sh
2️⃣ Run Deployment Script
bash k8s-deploy.sh
3️⃣ Deploy Database

Select:

1) Deploy DB

✔ Sets up storage
✔ Deploys MySQL StatefulSet
✔ Creates ClusterIP service

4️⃣ Deploy Application

Select:

2) Deploy App

✔ Deploys Spring Boot application
✔ Exposes service via NodePort

🌐 Application Access

Once deployed, access the application via:

http://<EC2-Public-IP>:<NodePort>

Example:

http://13.233.129.181:30085
📸 Screenshots
🌐 Application Running
<p align="center"> <img src="https://github.com/user-attachments/assets/a3ae3862-628f-4255-bd86-5060cd08b4ba" width="90%"> </p>
🐳 Docker Hub Repositories
<p align="center"> <img src="https://github.com/user-attachments/assets/10a095e0-0c1a-4f6f-bb59-9ba8aec95df5" width="90%"> </p>
🔍 Verification & Monitoring
# View all resources
kubectl get all -o wide

# Check application logs
kubectl logs -f deployment/spring-app

# Check services
kubectl get svc
# Verify the NodePort assignment
kubectl get svc spring-app-service
