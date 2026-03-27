🚀 Spring Boot & MySQL Deployment on Kubernetes (Kubeadm)
This project demonstrates a production-style deployment of a Spring Boot application with a MySQL database on a Kubernetes cluster provisioned using kubeadm. It highlights DevOps practices such as containerization, stateful orchestration, persistent storage, and automated deployment scripting.

🏗️ Architecture Overview
The deployment utilizes a multi-tier Kubernetes architecture:

Spring Boot: Deployed using a standard Deployment for scalability.

MySQL: Deployed using a StatefulSet to ensure stable network identity and data persistence.

Persistent Storage: Managed via local-path-provisioner with Persistent Volume Claims (PVC).

Networking: * ClusterIP: Used for secure, internal-only communication between the App and the DB.

NodePort: Used to expose the Spring Boot application to external users via the EC2 Public IP.

🧰 Technology Stack
Backend: Spring Boot (Java)

Database: MySQL 8.0

Containerization: Docker

Orchestration: Kubernetes (kubeadm)

Infrastructure: AWS EC2 (Ubuntu 22.04)

Scripting: Bash

📁 Project Structure
Bash
spring_mysql_kubeadm_sakcoorg/
│
├── kube_scripts/
│   ├── app-deploy-svc.yml      # Spring Boot Deployment & NodePort Service
│   ├── db-statefullset-svc.yml  # MySQL StatefulSet & ClusterIP Service
│   └── setup-storage.sh        # Storage Class & PV Provisioning
│
├── Dockerfile                  # Application Dockerization
├── k8s-deploy.sh               # Main Automation Script
├── docker_build_push.sh        # Image Build & Push Script
└── src/                        # Java Source Code
⚙️ Deployment Workflow
1. Build and Push Docker Image
Ensure your environment is logged into Docker Hub, then run:

Bash
bash docker_build_push.sh
The image will be available at: rohinicc/spring-kube-sakgroup

2. Run the Deployment Script
The k8s-deploy.sh script automates the entire process. Run it and follow the prompts:

Bash
bash k8s-deploy.sh
Step 1: Deploy DB (Sets up Storage, ClusterIP Service, and MySQL StatefulSet).

Step 2: Deploy App (Deploys Spring Boot and creates the NodePort Service).

🌐 Live Application Access
The application is reachable via the EC2 Public IP on the assigned NodePort.

URL: http://13.233.129.181:30085

Internal DB Connection: The app connects to MySQL via the service name mysql-service on port 3306 (ClusterIP).

📸 Screenshots
1. Application Running (Live)
The "SAK Group" portal successfully running on the Kubernetes cluster.

<p align="center">
<img src="https://github.com/user-attachments/assets/a3ae3862-628f-4255-bd86-5060cd08b4ba" width="90%">
</p>

2. Docker Hub Repositories
Verification of the containerized images in the Docker registry.

<p align="center">
<img src="https://github.com/user-attachments/assets/10a095e0-0c1a-4f6f-bb59-9ba8aec95df5" width="90%">
</p>

🔍 Verification Commands
Bash
# Check all resources in the namespace
kubectl get all

# Verify the NodePort assignment
kubectl get svc spring-app-service

# Check MySQL Pod persistence logs
kubectl logs -f mysql-0
