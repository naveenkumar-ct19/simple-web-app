pipeline {
    agent any

    environment {
        IMAGE_NAME = "naveen2182001/simple-web"
        IMAGE_TAG  = "latest"
        DOCKER_REGISTRY = "docker.io"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Cloning latest code from GitHub..."
                git branch: 'main',
                    url: 'https://github.com/naveenkumar-ct19/simple-web-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh """
                   docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                """
            }
        }

        stage('Push Docker Image') {
            environment {
                DOCKER_CREDS = credentials('dockerhub-creds')
            }
            steps {
                echo "Logging into Docker Hub..."
                sh """
                   echo ${DOCKER_CREDS_PSW} | docker login -u ${DOCKER_CREDS_USR} --password-stdin
                   docker push ${IMAGE_NAME}:${IMAGE_TAG}
                   docker logout
                """
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying application to Kubernetes..."
                sh """
                   kubectl apply -f deployment.yaml
                """
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed!"
        }
    }
}