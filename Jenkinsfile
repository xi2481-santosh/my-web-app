pipeline {
	agent any
	environment {
		DOCKER_HUB_CREDENTIALS_ID = 'nnksantosh'
		DOCKER_HUB_REPO = 'nnksantosh/web-app'
	}
	stages {
		stage('Checkout Github'){
			steps {
				git branch: 'main', credentialsId: 'xi2481-santosh', url: 'https://github.com/xi2481-santosh/my-web-app.git'
			}
		}		
		stage('Install node dependencies'){
			steps {
				sh 'npm install'
			}
		}
		stage('Test Code'){
			steps {
				sh 'npm test'
			}
		}
		stage('Build Docker Image'){
			steps {
				script {
					dockerImage = docker.build("${DOCKER_HUB_REPO}:latest")
				}
			}
		}
}
}
