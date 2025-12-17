pipeline {
    agent any

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
