pipeline {
    agent any

    environment {
        REGISTRY_AUTH_FILE = "${WORKSPACE}\\auth.json"
    }

    stages {

        stage('Build Image') {
            steps {
                bat 'podman build -t docker.io/naveen2182001/simple-web:latest .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat '''
                    if exist auth.json del auth.json
                    echo %DOCKER_PASS% | podman login registry-1.docker.io -u %DOCKER_USER% --password-stdin
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                bat 'podman push docker.io/naveen2182001/simple-web:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
