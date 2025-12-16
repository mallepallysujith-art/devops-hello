pipeline {
    agent any

    environment {
        IMAGE_NAME = "msujithreddy/devops-hello:latest"  // Your Docker Hub image
        SONARQUBE = "Sonarqube"                          // SonarQube name in Jenkins
        DOCKERHUB_CREDENTIALS = "dockerhub"             // Jenkins credentials ID for Docker Hub
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/mallepallysujith-art/devops-hello.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv(SONARQUBE) {
                        sh 'sonar-scanner'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t $IMAGE_NAME ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: "$DOCKERHUB_CREDENTIALS",
                                                  usernameVariable: 'DOCKER_USER',
                                                  passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $IMAGE_NAME
                    docker logout
                    """
                }
            }
        }

        stage('Deploy on EC2') {
            steps {
                echo "Deploying Docker container on EC2..."
                sh """
                docker stop devops-hello || true
                docker rm devops-hello || true
                docker run -d --name devops-hello -p 5000:5000 $IMAGE_NAME
                """
            }
        }

        // Optional Kubernetes stage
        /*
        stage('Deploy on Kubernetes') {
            steps {
                echo "Deploying to Kubernetes..."
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
        */
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check Jenkins logs!'
        }
    }
}




