pipeline{
    agent{
        label  'app'
        
    }

    environment {
        AWS_ACCOUNT_ID = '974328657523'
        AWS_REGION = 'us-east-1'
        ECR_REPO = 'node-app-repo'
    }

    stages {
        stage('Checkout Repository') {
            steps {
                git url: "$https://github.com/shreyachatterjee1418/example-voting-app/tree/main", branch: 'main'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}:latest")
                }

                sh '''
                aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:latest
                '''
            }
        }
    }
}
