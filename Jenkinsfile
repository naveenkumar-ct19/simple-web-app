pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/naveenkumar-ct19/simple-web-app.git'
            }
        }

        stage('Build Image') {
            steps {
                sh 'podman build -t docker.io/naveen2182001/simple-web:latest .'
            }
        }

        stage('Push Image') {
            steps {
                sh 'podman push docker.io/naveen2182001/simple-web:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
