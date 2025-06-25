pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/xi2481-santosh/my-web-app.git'
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
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t nnksantosh/my-web-app:latest .'
        withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'nnksantosh', passwordVariable: 'Swaraj@7620')]) {
          sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
          sh 'docker push nnksantosh/my-web-app:latest'
        }
      }
    }
    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f deployment.yaml'
        sh 'kubectl apply -f service.yaml'
      }
    }
  }
}
