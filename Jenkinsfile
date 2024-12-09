pipeline {
    agent {
        docker {
            image 'docker'
        }
    }
    environment {
        DOCKER_IMAGE = "jenkinsrepo"
        DOCKER_HUB_IMAGE = "sohaib22/jenkinsrepo"
    }
    stages {
        stage('Checkout Repository') {
            steps {
                git 'https://github.com/logicofsohaib/nginx-docker-project.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE}:latest .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                }
            }
        }
        stage('Push Docker Image to Docker Hub') {
            steps {
                sh 'docker tag ${DOCKER_IMAGE}:latest ${DOCKER_HUB_IMAGE}:latest'
                sh 'docker push ${DOCKER_HUB_IMAGE}:latest'
            }
        }
        stage('Deploy Docker Container') {
            steps {
                sh 'docker run -d -p 8080:80 ${DOCKER_HUB_IMAGE}:latest'
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
