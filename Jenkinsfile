pipeline {
    agent any

    environment {
        IMAGE_NAME = "msujithreddy/devops-hello:latest"  // Your Docker Hub image
        SONARQUBE = "sonar-scanner"                      // SonarQube name in Jenkins
        DOCKERHUB_CREDENTIALS = "dockerhub"              // Jenkins credentials ID for Docker Hub
    }

    stages {
        stage('Checkout') {
            steps {
                echo "=== Starting Checkout Stage ==="
                git branch: 'main', url: 'https://github.com/mallepallysujith-art/devops-hello.git'
                echo "=== Finished Checkout Stage ==="
            }
        }

        stage('Run Sonarqube') {
            environment {
                scannerHome = tool 'sonar-scanner';
            }
            steps {
                echo "=== Starting SonarQube Analysis Stage ==="
                withSonarQubeEnv(credentialsId: 'sonartoken', installationName: 'sonarqube') {
                    sh """
                    ${scannerHome}/bin/sonar-scanner \
                    -Dsonar.projectKey=Flask-HelloWorld-CI-CD \
                    -Dsonar.projectName=Flask-HelloWorld-CI-CD \
                    -Dsonar.sources=.
                    """
                }
                echo "=== Finished SonarQube Analysis Stage ==="
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "=== Starting Docker Build Stage ==="
                sh "docker build -t $IMAGE_NAME ."
                echo "=== Finished Docker Build Stage ==="
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "=== Starting Docker Push Stage ==="
                withCredentials([usernamePassword(credentialsId: "$DOCKERHUB_CREDENTIALS",
                                                  usernameVariable: 'DOCKER_USER',
                                                  passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $IMAGE_NAME
                    docker logout
                    """
                }
                echo "=== Finished Docker Push Stage ==="
            }
        }

        stage('Deploy on EC2') {
            steps {
                echo "=== Starting Deploy on EC2 Stage ==="
                sh """
                docker stop devops-hello || true
                docker rm devops-hello || true
                docker run -d --name devops-hello -p 5000:5000 $IMAGE_NAME
                """
                echo "=== Finished Deploy on EC2 Stage ==="
            }
        }

        // Optional Kubernetes stage
        /*
        stage('Deploy on Kubernetes') {
            steps {
                echo "=== Starting Kubernetes Deploy Stage ==="
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
                echo "=== Finished Kubernetes Deploy Stage ==="
            }
        }
        */
    }

    post {
        success {
            echo '=== Pipeline completed successfully! ==='
        }
        failure {
            echo '=== Pipeline failed. Check Jenkins logs! ==='
        }
    }
} 

