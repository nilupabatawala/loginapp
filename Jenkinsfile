pipeline {
    agent any
    environment {
        DOCKER_BUILDKIT = '1'
    }
    stages {
        stage('Checkout') {
            steps {// Checks out the project from the source control
                checkout scm
            }
    }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t loginapp:1 .'
            }
        }
    }
}