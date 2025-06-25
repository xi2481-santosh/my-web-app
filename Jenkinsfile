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
	git branch: 'main'
        url: 'https://github.com/xi2481-santosh/my-web-app.git'
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
    stage('Build and Tag Docker Image') {
      steps {
        sh "docker build -t ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${VERSION} -t ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:latest ."
        withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'xi2481-santosh', passwordVariable: 'Swaraj@7620')]) {
          sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
          sh "docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${VERSION}"
          sh "docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:latest"
        }
      }
    }
  }
}
