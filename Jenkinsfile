pipeline {
    agent any
    
     environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials') // Credential ID for Docker Hub
        DOCKER_IMAGE = 'bouzazi/tekrepo' // Your Docker Hub image name
    }
    
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:${env.BUILD_NUMBER}") // Tagging the image with Jenkins build number
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    withDockerRegistry([ credentialsId: "docker-hub-credentials", url: "https://index.docker.io/v1/" ]) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
