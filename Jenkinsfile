// Kalyani, Miloni, Kashish
// This is a Jenkins file which includes scripts for image building and uploading the image to docker hub. The last step is to deploy it on rancher
pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('20dced42-859d-4481-a508-cd17194a246e')
    }
    stages {
<<<<<<< HEAD
        stage('Timestamp') {
=======
        stage('Time Stamp') {
>>>>>>> 8067827ecf535705ef87b6f1da8497928f705e99
            steps {
                script {
                    // Defining a build timestamp variable
                    env.BUILD_TIMESTAMP = new Date().format("yyyyMMddHHmmss", TimeZone.getTimeZone('UTC'))
                    echo "Build timestamp: ${env.BUILD_TIMESTAMP}"
                }
            }
        }


<<<<<<< HEAD
        stage('Build docker image') {
=======
        stage('Docker Image Building') {
>>>>>>> 8067827ecf535705ef87b6f1da8497928f705e99
            steps {
                script {
                    
                    // Securely handling Docker login
                    withCredentials([usernamePassword(credentialsId: '20dced42-859d-4481-a508-cd17194a246e', 
                                                      usernameVariable: 'DOCKER_USER', 
                                                      passwordVariable: 'DOCKER_PASS')]) {
                        sh """
                            echo "\$DOCKER_PASS" | docker login -u "\$DOCKER_USER" --password-stdin
                        """
                    }

                    def imageName = "mshah32/img:${env.BUILD_TIMESTAMP}"
                    sh "docker build -t ${imageName} -f src/main/webapp/Dockerfile src/main/webapp"

                    env.IMAGE_NAME = imageName
                }
            }
        }
        

<<<<<<< HEAD
        stage('Push Image') {
=======
        stage('Pushing Image to DockerHub') {
>>>>>>> 8067827ecf535705ef87b6f1da8497928f705e99
            steps {
                script {
                    sh "docker push ${env.IMAGE_NAME}"
                }
            }
        }

<<<<<<< HEAD
        stage('Deployment') {
=======
        stage('Rancher Deploy') {
>>>>>>> 8067827ecf535705ef87b6f1da8497928f705e99
            steps {
                script {
                    sh "kubectl set image deployment/mshah32-deployment01 container-0=${env.IMAGE_NAME}"
                }
            }
        }
    }
}
