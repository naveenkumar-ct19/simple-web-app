pipeline {
    agent any

    environment {
        IMAGE_NAME = "docker.io/naveen2182001/simple-web:latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/naveenkumar-ct19/simple-web-app.git'
            }
        }

        stage('Build App Image') {
            steps {
                sh '''
                    pwd
                    ls -la
                    podman build --userns=host -t $IMAGE_NAME -f Dockerfile .
                '''
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
