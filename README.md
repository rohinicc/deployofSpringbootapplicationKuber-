# 🚀 Spring Boot & MySQL Deployment on Kubernetes (Kubeadm)

This repository contains a full-stack deployment of a Spring Boot application and a MySQL database on a Kubernetes cluster provisioned via **kubeadm**. It demonstrates a robust DevOps workflow including stateful orchestration, internal/external networking, and persistent storage management on AWS EC2.

---

## 🏗️ Architecture Overview

The deployment follows a decoupled, multi-tier architecture:

* **Application Layer:** Spring Boot (Java) managed via a Kubernetes `Deployment`.
* **Database Layer:** MySQL 8.0 managed via a `StatefulSet` for stable network identity.
* **Networking Strategy:**
    * **ClusterIP Service:** Provides a stable **internal** IP for the Spring Boot app to communicate with MySQL (secure and private).
    * **NodePort Service:** Exposes the Spring Boot application to **external** traffic via the EC2 Public IP on a specific port.
* **Storage:** Persistent Volume (PV) and Claims (PVC) using `local-path-provisioner`.

### 🔄 Architecture Flow

```text
User (Browser) 
      │
      ▼
[ EC2 Public IP : NodePort ]  <─── (NodePort Service)
      │
      ▼
[ Spring Boot Pod ]           <─── (Deployment)
      │
      ▼
[ MySQL Service ]             <─── (ClusterIP Service: Internal Only)
      │
      ▼
[ MySQL Pod ]                 <─── (StatefulSet)
      │
      ▼
[ Persistent Volume ]         <─── (Storage Layer)

🧰 Technology Stack
Backend: Spring Boot (Java)

Database: MySQL

Containerization: Docker

Orchestration: Kubernetes (kubeadm)

Infrastructure: AWS EC2 (Ubuntu 22.04)

Scripting: Bash

Gemini said
Markdown
# 🚀 Spring Boot & MySQL Deployment on Kubernetes (Kubeadm)

This repository contains a full-stack deployment of a Spring Boot application and a MySQL database on a Kubernetes cluster provisioned via **kubeadm**. It demonstrates a robust DevOps workflow including stateful orchestration, internal/external networking, and persistent storage management on AWS EC2.

---

## 🏗️ Architecture Overview

The deployment follows a decoupled, multi-tier architecture:

* **Application Layer:** Spring Boot (Java) managed via a Kubernetes `Deployment`.
* **Database Layer:** MySQL 8.0 managed via a `StatefulSet` for stable network identity.
* **Networking Strategy:**
    * **ClusterIP Service:** Provides a stable **internal** IP for the Spring Boot app to communicate with MySQL (secure and private).
    * **NodePort Service:** Exposes the Spring Boot application to **external** traffic via the EC2 Public IP on a specific port.
* **Storage:** Persistent Volume (PV) and Claims (PVC) using `local-path-provisioner`.

### 🔄 Architecture Flow

```text
User (Browser) 
      │
      ▼
[ EC2 Public IP : NodePort ]  <─── (NodePort Service)
      │
      ▼
[ Spring Boot Pod ]           <─── (Deployment)
      │
      ▼
[ MySQL Service ]             <─── (ClusterIP Service: Internal Only)
      │
      ▼
[ MySQL Pod ]                 <─── (StatefulSet)
      │
      ▼
[ Persistent Volume ]         <─── (Storage Layer)
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
│   ├── app-deploy-svc.yml      # Spring Boot Deployment & NodePort Service
│   ├── db-statefullset-svc.yml  # MySQL StatefulSet & ClusterIP Service
│   └── setup-storage.sh        # Script to configure StorageClass & PV
│
├── Dockerfile                  # Multi-stage Docker build
├── k8s-deploy.sh               # Main interactive deployment script
├── docker_build_push.sh        # Automation for Docker Hub image updates
└── src/                        # Java Application Source Code

⚙️ Deployment Workflow
1️⃣ Build & Push Docker Image
Ensure your environment is logged into Docker Hub, then execute:
bash docker_build_push.sh

2️⃣ Run Deployment Script
The k8s-deploy.sh script is an interactive tool to manage cluster resources:

Bash
bash k8s-deploy.sh
Option 1 (Deploy DB): Sets up storage, creates the ClusterIP service, and starts MySQL.

Option 2 (Deploy App): Deploys Spring Boot and exposes it via NodePort.

🌐 Live Application Access
Once deployed, the application is accessible via the web using the EC2 Public IP and the assigned NodePort.

Access URL: http://13.233.129.181:30085

Internal Communication: The Spring Boot backend connects to MySQL using the service name mysql-service (ClusterIP).

📸 Screenshots
1. Application Running (Live)
The "SAK Group" home page successfully served by Kubernetes pods.

<p align="center">
<img src="https://github.com/user-attachments/assets/a3ae3862-628f-4255-bd86-5060cd08b4ba" width="90%">
</p>

2. Docker Hub Repositories
Verification of the pushed images in the rohinicc registry.

<p align="center">
<img src="https://github.com/user-attachments/assets/10a095e0-0c1a-4f6f-bb59-9ba8aec95df5" width="90%">
</p>

🔍 Verification & Monitoring
Check the status of your Kubernetes resources with these commands:

Bash
# Check all Pods, Services, and StatefulSets
kubectl get all -o wide

# Check logs for the Spring Boot application
kubectl logs -f deployment/spring-app

# Verify the NodePort assignment
kubectl get svc spring-app-service
