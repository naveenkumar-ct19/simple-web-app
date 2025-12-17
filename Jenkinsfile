pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "your-dockerhub-username/simple-web-app:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                // Append /usr/local/bin to PATH safely
                withEnv(['PATH+DOCKER=/usr/local/bin']) {
                    sh 'docker --version'
                    sh "docker build -t $DOCKER_IMAGE ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withEnv(['PATH+DOCKER=/usr/local/bin']) {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-cred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                        sh "docker push $DOCKER_IMAGE"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withEnv(['PATH+KUBECTL=/usr/local/bin']) {
                    sh 'kubectl apply -f deployment.yaml'
                    sh 'kubectl apply -f service.yaml'
                }
            }
        }
    }
}
