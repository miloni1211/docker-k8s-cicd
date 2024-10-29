pipeline {
    agent any
    environment {
        // Replace 'my-dockerhub-password-id' with the ID of your DockerHub password credential in Jenkins
        DOCKERHUB_PASSWORD = credentials('Password@12345')
        // This sets a timestamp in the format "YYYYMMDD-HHMMSS" for tagging Docker images
        TIMESTAMP = new Date().format("20241029-044300")
    }
    stages {
        stage('Checkout') {
            steps {
                // Pull code from your Bitbucket repository
                git branch: 'master', url: 'https://github.com/miloni1211/docker-k8s-cicd.git' // Update this URL if your Bitbucket repo is different
            }
        }
        
        stage('Build') {
            steps {
                // Clean the workspace and compile your code
                sh 'mvn clean install'
                // If you have a packaging command, use it here. If not, comment out or remove this line
                // sh 'mvn package'
            }
        }
        
        stage('Docker Login and Tag') {
            steps {
                // Login to DockerHub using the credentials stored in Jenkins
                // Replace 'mydockeruser' with your actual DockerHub username
                sh 'echo $DOCKERHUB_PASSWORD | docker login -u mshah32 --password-stdin'
                
                // Tag the Docker image with the generated timestamp
                // Replace 'myapp' with your actual DockerHub repository/image name
                sh 'docker tag survey:latest myapp:$TIMESTAMP'
            }
        }
        
        stage('Push to DockerHub') {
            steps {
                // Push the newly tagged image to DockerHub
                // Replace 'myapp' with your actual DockerHub repository/image name
                sh 'docker push survey:$TIMESTAMP'
            }
        }
        
        stage('Update Kubernetes Deployment') {
            steps {
                // Use kubectl to update your deployment to use the new image tag
                // Replace 'my-app-deployment' with your Kubernetes deployment name
                // Replace 'app-container' with your container name in the Kubernetes deployment
                sh 'kubectl set image deployment/mshah32-deployment01 img=myapp:$TIMESTAMP'
            }
        }
    }
}
