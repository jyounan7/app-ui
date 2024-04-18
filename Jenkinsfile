pipeline {
    agent any
    
    environment {
        JFROG_CREDS = credentials('jcr')
        DOCKER_IMAGE_NAME = 'app'
        JFROG_URL = 'http://192.168.1.10:8081/artifactory'
        REPOSITORY_NAME = 'app'
    }
    
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Clone the Git repository
                    git 'https://github.com/jyounan7/app-ui.git'
                    
                    // Build Docker image
                    sh docker.build("${DOCKER_IMAGE_NAME}:latest", "-f Dockerfile .")
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

