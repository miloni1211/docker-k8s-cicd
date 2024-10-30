// Kalyani, Miloni, Kashish
// This is a Jenkins file which includes scripts for image building and uploading the image to docker hub. The last step is to deploy it on rancher
pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('20dced42-859d-4481-a508-cd17194a246e')
    }
    stages {
        stage('Time Stamp') {
            steps {
                script {
                    // Defining a build timestamp variable
                    env.BUILD_TIMESTAMP = new Date().format("yyyyMMddHHmmss", TimeZone.getTimeZone('UTC'))
                    echo "Build timestamp: ${env.BUILD_TIMESTAMP}"
                }
            }
        }


        stage('Docker Image Building') {
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
        

        stage('Pushing Image to DockerHub') {
            steps {
                script {
                    sh "docker push ${env.IMAGE_NAME}"
                }
            }
        }

        stage('Rancher Deploy') {
            steps {
                script {
                    sh "kubectl set image deployment/mshah32-deployment01 container-0=${env.IMAGE_NAME}"
                }
            }
        }
    }
}
