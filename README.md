DevOps Pipeline Project: Flask Hello World App
Project Overview
This project demonstrates a full DevOps pipeline:
Code stored in GitHub
Docker image built and pushed to Docker Hub
Deployment on an EC2 instance
Kubernetes deployment using Minikube
Publicly accessible Flask app through Docker and Kubernetes
Prerequisites
AWS account with EC2 instance (Ubuntu 24.04)
Docker installed on EC2
Git installed on EC2
Docker Hub account
Flask app with app.py and requirements.txt in GitHub repo
1️⃣ Docker Deployment on EC2
# Clone GitHub repository
git clone https://github.com/msujithreddy/devops-hello.git
cd devops-hello

# Build Docker image
docker build -t msujithreddy/devops-hello:latest .

# Login to Docker Hub
docker login

# Push Docker image
docker push msujithreddy/devops-hello:latest

# Run Docker container
docker run -d -p 5000:5000 msujithreddy/devops-hello:latest

# Verify container is running
docker ps
Access Flask app in browser:
http://100.48.72.194:5000 — should show Hello World from DevOps Pipeline!
2️⃣ Kubernetes Deployment with Minikube
Install Kubectl & Minikube
# Install kubectl
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client

# Install Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube version
Start Minikube Cluster
minikube start --driver=docker

# Check status
minikube status
kubectl get nodes
Deploy Flask App
# Apply Kubernetes manifests
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

# Check all pods
kubectl get pods -A

# Check services
kubectl get svc
Access Flask App via NodePort
# Get Minikube IP
minikube ip

# Combine with NodePort from 'kubectl get svc'
# Example:
# http://192.168.49.2:30007



http://100.48.72.194:5000    : this to Access Flask app in browser :it will show Hello World from devops pipline 


