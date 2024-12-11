pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'nginx-docker-image'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone your GitHub repository
                git 'https://github.com/logicofsohaib/mydockertesting.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container on port 8081 (since Jenkins is on 8080)
                    sh 'docker run -d -p 8081:80 ${DOCKER_IMAGE}:${DOCKER_TAG}'
                }
            }
        }

        stage('Test Deployment') {
            steps {
                script {
                    // Add commands for testing the deployed Nginx service if needed
                    echo 'Test Deployment (e.g., curl localhost:8081 or check container logs)'
                }
            }
        }
    }

    post {
        always {
            cleanWs()  // Clean workspace after pipeline runs
        }
    }
}
