pipeline {
    agent any
    environment {
        // Replace 'my-dockerhub-password-id' with the ID of your DockerHub password credential in Jenkins
        DOCKERHUB_PASSWORD = credentials('20dced42-859d-4481-a508-cd17194a246e')
        // This sets a timestamp in the format "YYYYMMDD-HHMMSS" for tagging Docker images
        TIMESTAMP = new Date().format("20241029-052500")
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
                script {
                    // Create an output directory for deployment
                    sh """
                        mkdir -p build-output

                        # Copy all necessary files and directories to the build output directory
                        cp -R src/main/webapp/* build-output/
                        cp -R src/main/webapp/META-INF build-output/
                        cp src/main/webapp/Dockerfile build-output/
                        cp -R src/main/webapp/HW1_Part2.html build-output/
                    """
                }
            }
        }
        
        stage('Docker Login and Tag') {
            steps {
                // Login to DockerHub using the credentials stored in Jenkins
                // Replace 'mydockeruser' with your actual DockerHub username
                sh 'echo $DOCKERHUB_PASSWORD | docker login -u mshah32@gmu.edu --password-stdin'
                
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
