pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t devops-web .'
            }
        }

        stage('Stop Old Container') {
            steps {
                bat 'docker stop devops-container || exit 0'
                bat 'docker rm devops-container || exit 0'
            }
        }

        stage('Deploy Container') {
            steps {
                bat 'docker run -d -p 8082:80 --name devops-container devops-web'
            }
        }
    }
}