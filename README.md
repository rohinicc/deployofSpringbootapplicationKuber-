# 🚀 Spring Boot & MySQL Deployment on Kubernetes (Kubeadm)

> A production-style Kubernetes deployment of a **Spring Boot** application backed by **MySQL 8.0**, provisioned on AWS EC2 using **kubeadm** — demonstrating stateful orchestration, persistent storage, and internal/external networking.

---

## 📋 Table of Contents

- [Architecture Overview](#-architecture-overview)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Prerequisites](#-prerequisites)
- [Deployment Workflow](#-deployment-workflow)
- [Live Application](#-live-application)
- [Verification & Monitoring](#-verification--monitoring)
- [Screenshots](#-screenshots)

---

## 🏗️ Architecture Overview

The deployment follows a **decoupled, multi-tier architecture** with clear separation between the application and database layers.

| Layer | Component | Kubernetes Resource |
|---|---|---|
| Application | Spring Boot (Java) | `Deployment` |
| Database | MySQL 8.0 | `StatefulSet` |
| Internal Network | MySQL access | `ClusterIP Service` |
| External Network | User access | `NodePort Service` |
| Storage | Data persistence | `PersistentVolume + PVC` |

### 🔄 Request Flow

```
User (Browser)
      │
      ▼
[ EC2 Public IP : NodePort ]     ◄── NodePort Service (External)
      │
      ▼
[ Spring Boot Pod ]              ◄── Deployment
      │
      ▼
[ MySQL Service : mysql-service ]◄── ClusterIP Service (Internal Only)
      │
      ▼
[ MySQL Pod ]                    ◄── StatefulSet
      │
      ▼
[ Persistent Volume ]            ◄── local-path-provisioner
```

- **ClusterIP** keeps MySQL private — only accessible within the cluster.
- **NodePort** exposes Spring Boot to the internet via EC2's public IP.
- **StatefulSet** ensures MySQL gets a stable network identity and persistent storage across restarts.

---

## 🛠️ Tech Stack

- **Application:** Java, Spring Boot
- **Database:** MySQL 8.0
- **Containerization:** Docker (multi-stage build)
- **Orchestration:** Kubernetes (kubeadm)
- **Cloud:** AWS EC2
- **Storage:** local-path-provisioner (PV + PVC)
- **Registry:** Docker Hub (`rohinicc/spring-kube-sakgroup`)

---

## 📁 Project Structure

```
spring_mysql_kubeadm_sakcoorg/
│
├── kube_scripts/
│   ├── app-deploy-svc.yml       # Spring Boot Deployment & NodePort Service
│   ├── db-statefullset-svc.yml  # MySQL StatefulSet & ClusterIP Service
│   └── setup-storage.sh         # StorageClass & PersistentVolume configuration
│
├── Dockerfile                   # Multi-stage Docker build
├── k8s-deploy.sh                # Interactive deployment script
├── docker_build_push.sh         # Docker Hub build & push automation
└── src/                         # Java Application Source Code
```

---

## ✅ Prerequisites

- AWS EC2 instances with kubeadm cluster initialized (1 master + worker nodes)
- `kubectl` configured and pointing to your cluster
- Docker installed and logged into Docker Hub
- EC2 Security Group allows inbound traffic on the NodePort (e.g., `30085`)

---

## ⚙️ Deployment Workflow

### 1️⃣ Build & Push Docker Image

```bash
bash docker_build_push.sh
```

This builds the Spring Boot app image and pushes it to Docker Hub.

> **Target Image:** `rohinicc/spring-kube-sakgroup`

---

### 2️⃣ Run the Interactive Deployment Script

```bash
bash k8s-deploy.sh
```

The script presents two options:

| Option | Action |
|---|---|
| `1` — Deploy DB | Runs `setup-storage.sh`, creates PV/PVC, deploys MySQL `StatefulSet` + `ClusterIP Service` |
| `2` — Deploy App | Deploys Spring Boot `Deployment` + `NodePort Service` |

> **Always deploy the DB (Option 1) before the App (Option 2).**

---

### 3️⃣ Manual Apply (Alternative)

```bash
# Step 1: Set up storage
bash kube_scripts/setup-storage.sh

# Step 2: Deploy MySQL
kubectl apply -f kube_scripts/db-statefullset-svc.yml

# Step 3: Deploy Spring Boot
kubectl apply -f kube_scripts/app-deploy-svc.yml
```

---

## 🌐 Live Application

Once deployed, access the application at:

```
http://13.233.129.181:30085
```

| Detail | Value |
|---|---|
| External URL | `http://<EC2-Public-IP>:30085` |
| Internal DB Host | `mysql-service` (ClusterIP) |
| Docker Image | `rohinicc/spring-kube-sakgroup` |

> The Spring Boot app connects to MySQL using the Kubernetes DNS name `mysql-service` — no hardcoded IPs required.

---

## 🔍 Verification & Monitoring

```bash
# View all running pods, services, and StatefulSets
kubectl get all -o wide

# Stream logs from the Spring Boot deployment
kubectl logs -f deployment/spring-app

# Check NodePort assignment for the Spring Boot service
kubectl get svc spring-app-service

# Check MySQL StatefulSet and pod health
kubectl get statefulset
kubectl get pods -l app=mysql

# Describe PersistentVolumeClaim status
kubectl get pvc
```

---

## 📸 Screenshots

### Application Running (Live)
The SAK Group home page successfully served by Kubernetes pods.

<p align="center">
  <img src="https://github.com/user-attachments/assets/a3ae3862-628f-4255-bd86-5060cd08b4ba" width="90%" alt="Spring Boot App Running on Kubernetes"/>
</p>

### Docker Hub Repository
Verification of the pushed image in the `rohinicc` registry.

<p align="center">
  <img src="https://github.com/user-attachments/assets/10a095e0-0c1a-4f6f-bb59-9ba8aec95df5" width="90%" alt="Docker Hub - rohinicc registry"/>
</p>

---

## 👩‍💻 Author

**Rohini C**
- GitHub: [@rohinicc](https://github.com/rohinicc)
- LinkedIn: [linkedin.com/in/rohini-c-na16](https://linkedin.com/in/rohini-c-na16)
- Portfolio: [rohinijuji.onrender.com](https://rohinijuji.onrender.com)

---

> ⭐ If you found this project helpful, consider giving it a star!
