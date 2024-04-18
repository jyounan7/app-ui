pipeline{

	agent any

	 environment {
        JFROG_CREDS = credentials('jcr')
        DOCKER_IMAGE_NAME = 'app'
        JFROG_URL = 'http://192.168.1.10:8081/artifactory'
        REPOSITORY_NAME = 'app'
    }}

	stages {
	    	stage('gitclone') {

			steps {
				git 'https://github.com/YasserAhmedMoh/Docker_Demo.git'
			}
		}
		stage('Build') {

			steps {
				
                sh docker.build("${DOCKER_IMAGE_NAME}:latest" .)
			}
		}

	
