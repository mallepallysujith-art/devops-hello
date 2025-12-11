pipeline {
    agent any

    environment {
        IMAGE_NAME = "msujithreddy/devops-hello"
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/mallepallysujith-art/devops-hello.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker build -t $IMAGE_NAME .
                        docker push $IMAGE_NAME
                        docker logout
                        """
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
            }
        }
    }
}



