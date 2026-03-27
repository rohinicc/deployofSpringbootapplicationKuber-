# 🚀 Spring Boot & MySQL Deployment on Kubernetes (Kubeadm)

This repository contains a production-style deployment of a Spring Boot application with a MySQL database on a Kubernetes cluster provisioned using kubeadm.

It demonstrates containerization, orchestration, persistent storage, networking, and deployment automation on AWS EC2.

---

## 🏗️ Architecture Overview

The system follows a multi-tier, decoupled architecture:

- Application Layer → Spring Boot (Deployment)
- Database Layer → MySQL (StatefulSet)
- Networking → ClusterIP (internal) + NodePort (external)
- Storage → Persistent Volume (PV) + PVC

---

## 🔄 Architecture Flow

```text
User (Browser)
      │
      ▼
EC2 Public IP : NodePort
      │
      ▼
Kubernetes NodePort Service
      │
      ▼
Spring Boot Pod (Deployment)
      │
      ▼
MySQL Service (ClusterIP - Internal)
      │
      ▼
MySQL Pod (StatefulSet)
      │
      ▼
Persistent Volume (Storage)
🧰 Technology Stack
Backend           → Spring Boot (Java)
Database          → MySQL
Containerization  → Docker
Orchestration     → Kubernetes (kubeadm)
Infrastructure    → AWS EC2 (Ubuntu 22.04)
Scripting         → Bash
📁 Project Structure
spring_mysql_kubeadm_sakcoorg/
│
├── kube_scripts/
│   ├── app-deploy-svc.yml
│   ├── db-statefullset-svc.yml
│   └── setup-storage.sh
│
├── Dockerfile
├── k8s-deploy.sh
├── docker_build_push.sh
└── src/
⚙️ Deployment Workflow
Step 1 → Build Docker Image
         bash docker_build_push.sh

Step 2 → Run Deployment Script
         bash k8s-deploy.sh

Step 3 → Deploy Database
         Select → 1

Step 4 → Deploy Application
         Select → 2
