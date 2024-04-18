pipeline {
    agent any
    
    environment {
        JFROG_CREDS = credentials('jcr')
        DOCKER_IMAGE_NAME = 'test'
        JFROG_URL = 'http://192.168.1.10:8081/artifactory'
        REPOSITORY_NAME = 'app-ui'
    }
    
    stages {
        stage('Build Docker Image') {
            steps {
                git 'https://github.com/jyounan7/app-ui.git'
                script {
                    // Build Docker image
                    docker.build("${DOCKER_IMAGE_NAME}:latest", "-f Dockerfile .")
                }
            }
        }
        
        stage('Tag and Push to JFrog Artifactory') {
            steps {
                script {
                    // Tag Docker image
                    docker.image("${DOCKER_IMAGE_NAME}:latest").tag("${JFROG_URL}/${REPOSITORY_NAME}/${DOCKER_IMAGE_NAME}:latest")
                    
                    // Push Docker image to JFrog Artifactory
                    docker.withRegistry(JFROG_URL, JFROG_CREDS) {
                        docker.image("${JFROG_URL}/${REPOSITORY_NAME}/${DOCKER_IMAGE_NAME}:latest").push()
                    }
                }
            }
        }
    }
}
