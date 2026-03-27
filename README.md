<img width="2867" height="1615" alt="Screenshot 2026-03-27 131542" src="https://github.com/user-attachments/assets/22cb1679-e23e-446f-8a17-76bc01cc6cc5" />🚀 Spring Boot & MySQL Deployment on Kubernetes (Kubeadm)

This project showcases a production-style deployment of a Spring Boot application with a MySQL database on a Kubernetes cluster provisioned using kubeadm.

It demonstrates core DevOps practices including containerization, orchestration, persistent storage, and automated deployment scripting.

🧰 Technology Stack
Backend: Spring Boot (Java)
Database: MySQL
Containerization: Docker
Orchestration: Kubernetes (kubeadm)
Scripting: Bash
Infrastructure: AWS EC2 (Linux)
🏗️ Architecture Overview
Spring Boot deployed using Kubernetes Deployment
MySQL deployed using StatefulSet (ensures stable storage & identity)
Persistent storage via local-path-provisioner
Application exposed using NodePort Service
Docker images hosted on Docker Hub
📁 Project Structure
spring_mysql_kubeadm_sakcoorg/
│
├── kube_scripts/
│   ├── app-deploy-svc.yml        # Spring Boot Deployment + Service
│   ├── db-statefullset-svc.yml  # MySQL StatefulSet + Service
│   └── setup-storage.sh        # Storage Setup Script
│
├── Dockerfile                  # Application Image
├── k8s-deploy.sh              # Deployment Automation Script
├── docker_build_push.sh       # Build & Push Script
├── pom.xml                    # Maven Configuration
└── src/                       # Source Code
⚙️ Deployment Workflow

1️⃣ Clone Repository
git clone <your-repo-url>
cd spring_mysql_kubeadm_sakcoorg
2️⃣ Build & Push Docker Image
bash docker_build_push.sh

Ensure the image is pushed to Docker Hub:

rohinicc/spring-kube-sakgroup
3️⃣ Configure Deployment Script

Update the base path in k8s-deploy.sh:

BASE_PATH="$(pwd)/kube_scripts"

This ensures correct file resolution during execution.

4️⃣ Run Deployment Script
bash k8s-deploy.sh
5️⃣ Deploy Database (MySQL)

Select:

1) Deploy DB

✔ Initializes storage
✔ Deploys MySQL StatefulSet
✔ Creates internal service

6️⃣ Deploy Application

Select:

2) Deploy App

✔ Deploys Spring Boot app
✔ Exposes via NodePort

7️⃣ Verify Deployment
kubectl get pods
kubectl get svc
8️⃣ Access Application

Open in browser:

http://<EC2-Public-IP>:<NodePort>
📸 Screenshots
🌐 Application Running
<img width="2867" height="1615" alt="Screenshot 2026-03-27 131542" src="https://github.com/user-attachments/assets/a3ae3862-628f-4255-bd86-5060cd08b4ba" />

🐳 Docker Hub Repositories
<img width="2871" height="1723" alt="Screenshot 2026-03-27 131608" src="https://github.com/user-attachments/assets/10a095e0-0c1a-4f6f-bb59-9ba8aec95df5" />

✅ Key Features
Automated Kubernetes deployment via Bash script
MySQL deployed using StatefulSet for persistence
Dynamic storage provisioning
NodePort service exposure
Docker image build & push automation
Modular Kubernetes YAML configurations


🧠 DevOps Concepts Demonstrated
Containerization using Docker
Kubernetes workloads (Deployment vs StatefulSet)
Service exposure and networking
Persistent volume provisioning
Infrastructure automation
Debugging deployment issues


🔧 Challenges & Solutions
Challenge	Solution
Incorrect script path	Used dynamic path ($(pwd))
Storage setup issues	Automated with setup script
Service accessibility	Configured NodePort

🚀 Future Improvements
Add Ingress Controller for domain-based routing
Implement CI/CD pipeline (Jenkins/GitHub Actions)
Use Helm charts for scalable deployments
Integrate monitoring (Prometheus + Grafana)
Enable Horizontal Pod Autoscaling (HPA)

👩‍💻 Author

Rohini

⭐ If you find this project useful, consider starring the repository!
