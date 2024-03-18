pipeline {
    agent any
    environment {
        DOCKER_BUILDKIT = '1'
        DOCKER_CLI_EXPERIMENTAL= 'enabled'
    }
    stages {
        stage('Checkout') {
            steps {// Checks out the project from the source control
                checkout scm
            }
    }
        stage('Build Docker Image') {
            steps {
                sh 'docker buildx build -t loginapp:1 .'
            }
        }
    }
}