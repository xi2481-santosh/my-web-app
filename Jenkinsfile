pipeline {
    agent any

    environment {
        USER = 'nnksantosh'
        VERSION = '2.1.1'
        IMAGE_NAME = "${USER}/myapp:${VERSION}"
    }

    stages {
        stage("Code Checkout") {
            steps {
                echo 'Code Testing'
                git url: "https://github.com/xi2481-santosh/my-web-app.git", branch: "main"
            }
        }

        stage("Build Docker Image") {
            steps {
                echo 'Code Building'
                sh 'docker build -t myapp .'
            }
        }

        stage("Trivy Image Scan") {
            steps {
                script {  
            
		echo "Scanning Docker image with Trivy and saving report..."
            sh """
            docker tag myapp ${IMAGE_NAME}
            docker run --rm \
              -v /var/run/docker.sock:/var/run/docker.sock \
              -v \$HOME/.cache:/root/.cache \
              -v \$(pwd):/report \
              aquasec/trivy image \
                --severity HIGH,CRITICAL \
                --format table \
                --output /report/trivy-report.txt \
                ${IMAGE_NAME}
            """
                }
            }
        }

        stage("Push to Docker Hub") {
            steps {
                echo 'Push to DockerHub'
                withCredentials([usernamePassword(credentialsId: "dockerhub", passwordVariable: 'DOCKERHUB_PASS', usernameVariable: 'DOCKERHUB_USER')]) {
                    sh """
                    echo "${DOCKERHUB_PASS}" | docker login -u "${DOCKERHUB_USER}" --password-stdin
                    docker tag myapp ${DOCKERHUB_USER}/myapp:${VERSION}
                    docker push ${DOCKERHUB_USER}/myapp:${VERSION}
                    """
                }
            }
        }

        stage("Deploy to Container") {
            steps {
                echo 'Deploying to container'
                sh "docker run -d -p 80:80 ${USER}/myapp:${VERSION}"
            }
        }
    }
}

