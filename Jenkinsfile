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

        stage('Deploy Container') {
            steps {
                script {
                    // Pull the image from Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                        dockerImage = docker.image("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                        dockerImage.pull()
                    }
                    
                    // Run docker-compose up to start the container
                    sh 'docker-compose up -d' // Modify this command based on your docker-compose setup
                }
            }
        }
    }
}
