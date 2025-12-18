# DevOps CI/CD Project: Automated Deployment of Flask App

## Project Overview
This project demonstrates a **full CI/CD pipeline** for a simple Flask application. It includes:

- **Continuous Integration (CI):** Automated code quality checks using **SonarQube**
- **Continuous Deployment (CD):** Automated building of Docker images and deployment to **Kubernetes (k3s)** on AWS EC2
- **End-to-End Automation:** GitHub → Jenkins → SonarQube → Docker → Kubernetes → EC2


---

## Tech Stack & Tools
| Layer | Tools/Technologies |
|-------|------------------|
| Source Code Management | GitHub |
| CI/CD | Jenkins |
| Code Quality | SonarQube |
| Containerization | Docker |
| Container Orchestration | Kubernetes (k3s) |
| Cloud Provider | AWS EC2 |
| Application | Python Flask |

---

## Architecture / Flow Diagram
GitHub (Code Commit)
↓
Jenkins Pipeline
↓
SonarQube Analysis (Quality Gate)
↓
Docker Image Build & Push (Docker Hub)
↓
Kubernetes Deployment (k3s on EC2)
↓
Flask App accessible via NodePort


---

## Step 1: Set Up EC2 Instance
1. Launch an AWS EC2 instance  
2. Connect via SSH:

```bash
ssh -i "devops-key.pem" ubuntu@<EC2_PUBLIC_IP>


sudo apt update -y && sudo apt upgrade -y
sudo apt install -y openjdk-11-jdk git curl wget unzip docker.io docker-compose
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER

Install Jenkins:

sudo docker run -d -p 8080:8080 -p 50000:50000 --name jenkins jenkins/jenkins:lts
sudo docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
Access Jenkins: http://<EC2_PUBLIC_IP>:8080
Install recommended plugins and create admin user

Install SonarQube:

sudo docker run -d --name sonarqube -p 9000:9000 sonarqube:lts
Access SonarQube: http://<EC2_PUBLIC_IP>:9000
Use Quality Gate to check code quality


Dockerize Flask App:
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python3", "app.py"]

Build and push Docker image:
docker build -t msujithreddy/devops-hello:latest .
docker login
docker push msujithreddy/devops-hello:latest

Install k3s (Lightweight Kubernetes) i used k3s insted of k8s its the same but k3s are lightweight cus im useing t3.small its small instance 

curl -sfL https://get.k3s.io | sh -
sudo systemctl status k3s

Configure kubectl:

sudo cp /etc/rancher/k3s/k3s.yaml $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
export KUBECONFIG=$HOME/.kube/config
echo "export KUBECONFIG=\$HOME/.kube/config" >> ~/.bashrc
source ~/.bashrc
kubectl get nodes

Kubernetes Deployment: flask-deployment.yaml
Apply Deployment:
kubectl apply -f flask-deployment.yaml

Verify Pods and Service:
kubectl get pods
kubectl get svc
Pod STATUS = Running
Service NodePort = 30007
Access Flask app in browser:
http://<EC2_PUBLIC_IP>:30007

