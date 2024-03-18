pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {// Checks out the project from the source control
                checkout scm
            }
    }
        stage('Build Docker Image') {
            steps {
                sh 'docker --version'
                sh 'docker build -t  loginapp:1 .'
            }
        }
    }
}