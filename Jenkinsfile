pipeline {
    agent any
    
    environment {
        JFROG_CREDS = credentials('jcr')
        DOCKER_IMAGE_NAME = 'app'
        JFROG_URL = 'http://192.168.1.10:8081/artifactory'
        REPOSITORY_NAME = 'app'
    }
    
    stages {
        stage('gitclone') {
            steps {
                git 'https://github.com/jyounan7/app-ui.git'
            }
        }
        
        stage('Build') {
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t app/test:latest .
                }
            }
        }
        
        stage('Push to JFrog Artifactory') {
            steps {
                script {
                    // Tag Docker image
                    def dockerImageTag = "${JFROG_URL}/${REPOSITORY_NAME}/${DOCKER_IMAGE_NAME}:latest"
                    docker.image("${DOCKER_IMAGE_NAME}:latest").tag(dockerImageTag)
                    
                    // Log in to JFrog Container Registry
                    withCredentials([usernamePassword(credentialsId: 'JFROG_CREDS', passwordVariable: 'JFROG_PASSWORD', usernameVariable: 'JFROG_USERNAME')]) {
                        sh "docker login -u ${JFROG_USERNAME} -p ${JFROG_PASSWORD} ${JFROG_URL}"
                    }
                    
                    // Push Docker image to JFrog Container Registry
                    sh "docker push ${dockerImageTag}"
                }
            }
        }
    }
}
