node {
    def ecrRepository

    stage('Checkout') {
        // Checks out the project from the source control
        checkout scm
    }

    stage('Build Docker Image') {
        /* 
        Assuming Dockerfile is at the root of the project directory. 
        Replace 'your-application' with the name of your application/image.
        */
        sh "docker build -t loginapp:${env.BUILD_ID} ."
    }

    stage('Login to AWS ECR') {
        /* 
        Replace 'aws_access_key_id', 'aws_secret_access_key', and 'your-region' with your AWS credentials and region.
        */
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws_credentials']]) {
            sh 'aws ecr get-login-password --region eu-north-1 | docker login --username AWS --password-stdin 424416207406.dkr.ecr.eu-north-1.amazonaws.com'
        }
    }

    stage('Tag and Push to ECR') {
        /* 
        Replace 'your-aws-account-id.dkr.ecr.your-region.amazonaws.com/your-repo-name' with your ECR repository path.
        */
        ecrRepository = '424416207406.dkr.ecr.eu-north-1.amazonaws.com/loginapp'
        //def imageTag = "${ecrRepository}:${env.BUILD_ID}"
        //app.tag(imageTag).push()

        sh "docker tag loginapp:${env.BUILD_ID} ${ecrRepository}:${env.BUILD_ID}"
        sh "docker push 424416207406.dkr.ecr.eu-north-1.amazonaws.com/loginapp:${env.BUILD_ID}"
        }
   

    stage('Clean Up') {
        // Clean up Docker images to prevent the Jenkins node from running out of space
        sh "docker rmi -f \$(docker images -q loginapp:${env.BUILD_ID})"
        //sh "docker rmi -f \$(docker images -q ${ecrRepository}:${env.BUILD_ID})"
    }¨

    stage('Update yaml') {            
        // Use sed to replace $IMG_TAG with Jenkins BUILD_ID in the deployment file
        sh "sed -i 's/\\\$IMG_TAG/${env.BUILD_ID}/g' manifests/deployment-file.yaml"
    }

    stage('Deploy to EKS') {

         withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws_credentials']]) {
             withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                sh "kubectl apply -f manifests/deployment.yaml"

            }
        }

       
    }
}
