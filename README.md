# DevOps Pipeline Project: Flask Hello World App

## Project Overview
This project demonstrates a full DevOps pipeline:
- Code stored in GitHub
- Docker image built and pushed to Docker Hub
- Deployment on an EC2 instance
- Publicly accessible Flask app through Docker

## Prerequisites
- AWS account with EC2 instance (Ubuntu 24.04)
- Docker installed on EC2
- Git installed on EC2
- Docker Hub account
- Flask app with `app.py` and `requirements.txt` in GitHub repo

## Step-by-Step Commands

### 1. Clone GitHub repository
```bash
git clone https://github.com/msujithreddy/devops-hello.git
cd devops-hello


docker build -t msujithreddy/devops-hello:latest .

docker login

docker push msujithreddy/devops-hello:latest

docker run -d -p 5000:5000 msujithreddy/devops-hello:latest

docker ps

http://100.48.72.194:5000    : this to Access Flask app in browser :it will show Hello World from devops pipline 


