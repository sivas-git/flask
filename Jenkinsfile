pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = 'docker-hub'  // The Jenkins credentials ID for Docker Hub
        DOCKER_IMAGE_NAME = 'siva3r/flask'           // Replace with your Docker Hub username and repo name
        IMAGE_TAG = "${env.BUILD_NUMBER}"                   // Use the Jenkins build number as the image tag
    }    
    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/sivas-git/flask.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com', DOCKER_HUB_CREDENTIALS) {
                        docker.image("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}").push()
                    }
                }
            }
        }
        stage('Deploy Application') {
            steps {
                sh 'docker run -d -p 5000:5000 ${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
