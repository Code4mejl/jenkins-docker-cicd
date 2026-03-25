pipeline {

    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/Code4mejl/jenkins-docker-cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t mywebsite .'
            }
        }

        stage('Deploy Container') {
            steps {
                bat 'docker rm -f devops-container || exit 0'
                bat 'docker run -d -p 8082:80 --name devops-container mywebsite'
            }
        }

    }
}