pipeline {
    agent any

    environment {
        IMAGE_NAME = "docker.io/naveen2182001/simple-web"
        IMAGE_TAG  = "latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/naveenkumar-ct19/simple-web-app.git'
            }
        }

        stage('Build Image (Podman)') {
            steps {
                bat "podman build -t %IMAGE_NAME%:%IMAGE_TAG% ."
            }
        }

        stage('Push Image (Podman)') {
            environment {
                REG_CREDS = credentials('dockerhub-creds')
            }
            steps {
                bat """
                podman login docker.io -u %REG_CREDS_USR% -p %REG_CREDS_PSW%
                podman push %IMAGE_NAME%:%IMAGE_TAG%
                """
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat "kubectl apply -f deployment.yaml"
            }
        }
    }
}
