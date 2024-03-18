node {
    def app
    
    environment {
        DOCKER_BUILDKIT = '1'
    }
    

    stage('Checkout') {
        // Checks out the project from the source control
        checkout scm
    }

    stage('Build Docker Image') {
        /* 
        Assuming Dockerfile is at the root of the project directory. 
        Replace 'your-application' with the name of your application/image.
        */
        app = docker.build("loginapp:${env.BUILD_ID}")
    }

    stage('Login to AWS ECR') {
        /* 
        Replace 'aws_access_key_id', 'aws_secret_access_key', and 'your-region' with your AWS credentials and region.
        */
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws_credentials']]) {
            sh 'eval $(aws ecr get-login --region eu-north-1 --no-include-email)'
        }
    }

    stage('Tag and Push to ECR') {
        /* 
        Replace 'your-aws-account-id.dkr.ecr.your-region.amazonaws.com/your-repo-name' with your ECR repository path.
        */
        def ecrRepository = '424416207406.dkr.ecr.eu-north-1.amazonaws.com/loginapp'
        def imageTag = "${ecrRepository}:${env.BUILD_ID}"
        app.tag(imageTag).push()
    }

    stage('Clean Up') {
        // Clean up Docker images to prevent the Jenkins node from running out of space
        sh "docker rmi \$(docker images -q loginapp:${env.BUILD_ID})"
        sh "docker rmi \$(docker images -q ${ecrRepository}:${env.BUILD_ID})"
    }
}
