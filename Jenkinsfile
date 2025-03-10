pipeline {
    agent any

    environment {
        DOCKER_USER = "your-dockerhub-username"
        DOCKER_PASS = credentials("DOCKER_HUB_CREDENTIALS")
        IMAGE_NAME = "${DOCKER_USER}/cicd-flask-app"
        IMAGE_TAG = "latest"
    }

    stages {
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
        
        stage("Checkout Code") {
            steps {
                git branch: 'main', url: 'https://github.com/yourusername/cicd-flask-app.git'
            }
        }

        stage("Build Docker Image") {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage("Push Docker Image to Docker Hub") {
            steps {
                script {
                    sh """
                    echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin
                    docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Docker Image pushed successfully!"
        }
        failure {
            echo "Build or push failed!"
        }
    }
}
