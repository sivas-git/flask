pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/sivas-git/flask.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("flask:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'Dockerhub-credentials') {
                        docker.image("flask:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
        stage('Deploy Application') {
            steps {
                sh 'docker run -d -p 5000:5000 flask:${env.BUILD_NUMBER}'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
