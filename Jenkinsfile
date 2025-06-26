pipeline {
    agent any
    environment {
    USER= 'nnksantosh'
    VERSION = '2.1'
}
  
    stages {
        stage("code") {
            steps {
                echo 'Code Testing'
                git url: "https://github.com/xi2481-santosh/my-web-app.git", branch: "main"
            }
		
		script {
                    env.COMMIT_ID = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                }
		
		echo $COMMIT_ID
        }
        stage("Build") {
            steps {
                echo 'Code Building'
                sh 'docker build -t myapp .'
            }
        }
        stage("Push to dockerHub") {
            steps {
                echo 'Push to dockerHub'
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]){
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh "docker tag myapp ${env.dockerhubuser}/myapp:$VERSION"
                sh "docker push ${env.dockerhubuser}/myapp:$VERSION"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo 'Deploying to container'
		sh "docker run -d -p 80:80 $USER/myapp:$VERSION"
            }
        }
    }
}

