pipeline {
    agent any

    environment {
        IMAGE_NAME = "docker.io/naveen2182001/simple-web:latest"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                deleteDir() // cleans old files
            }
        }

        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          userRemoteConfigs: [[
                              url: 'https://github.com/naveenkumar-ct19/simple-web-app.git'
                          ]]])
            }
        }

        stage('Build Image') {
            steps {
                sh 'sudo podman build -t $IMAGE_NAME . || podman build -t $IMAGE_NAME .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo "$DOCKER_PASS" | podman login docker.io -u "$DOCKER_USER" --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'podman push $IMAGE_NAME'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl version --client
                    kubectl apply -f deployment.yaml
                '''
            }
        }
    }
}
