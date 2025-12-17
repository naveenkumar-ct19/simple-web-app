# simple-web-app
Simple web application for CI/CD testing
End-End pipeline





1. GitHub Repository Setup
Create a GitHub repository containing your simple web server application (e.g., a basic Node.js or Python Flask app) - You can use your private github as you are using non-cisco code.
Include a Dockerfile to containerize the application.
Add Kubernetes deployment and service YAML files to deploy the app on Kubernetes.
Add a Jenkinsfile defining the CI/CD pipeline stages.

2. Jenkins Configuration
Create a Jenkins pipeline job configured to use your GitHub repository as the source.
Configure Jenkins credentials with a GitHub personal access token for repository access.
Set up a GitHub webhook to trigger Jenkins builds automatically on code push:
    - In GitHub repo settings, add a webhook with the Jenkins webhook URL
In Jenkins job configuration:
    - Under Source Code Management, select Git and provide repo details.
    - Under Build Triggers, enable "GitHub hook trigger for GITScm polling"

3. Jenkinsfile Pipeline Stages (Declarative Pipeline)
Checkout: Clone or pull the latest code from GitHub.
Build Docker Image: Build the Docker image using the Dockerfile.
Push Docker Image: Push the image to a container registry (Docker Hub, private registry, or local).
Deploy to Kubernetes: Apply Kubernetes manifests (kubectl apply -f deployment.yaml) to update the deployment.
Optional: Run tests or SonarQube analysis as part of the pipeline.

4. Kubernetes Setup
Use a local Kubernetes cluster (e.g., Minikube or Docker Desktop Kubernetes).
Ensure kubectl is configured to interact with your cluster.
Create Kubernetes deployment and service YAML files for your web server.
Use Kubernetes Secrets for sensitive data if needed, following best practices

5. Running the Pipeline
Commit and push code changes to GitHub.
Jenkins automatically triggers the pipeline via webhook.
The pipeline builds, pushes the Docker image, and deploys the updated app to Kubernetes.