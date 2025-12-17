pipeline {
    agent any

    environment {
        IMAGE_NAME = "docker.io/naveen2182001/simple-web:latest"
    }

    stages {

        stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Check Tools') {
            steps {
                sh '''
                docker --version
                kubectl version --client
                minikube status
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                sh '''
                docker push $IMAGE_NAME
                '''
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh '''
                kubectl apply -f deployment.yaml
                kubectl rollout status deployment/simple-web
                '''
            }
        }
    }
}
