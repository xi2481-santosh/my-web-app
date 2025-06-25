pipeline {
  agent any
  environment {
    DOCKER_IMAGE = "my-new-web-app"
    DOCKER_REGISTRY = "xi2481-santosh"
    VERSION = "${env.BUILD_ID}" // Uses Jenkins build number as version
  }
  stages {
    stage('Checkout') {
      steps {
		git branch: 'main', credentialsId: 'xi2481-santosh', url: 'https://github.com/xi2481-santosh/my-web-app.git'
      }
    }
    stage('Build') {
      steps {
        sh 'npm install'
      }
    }
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }
}
}
