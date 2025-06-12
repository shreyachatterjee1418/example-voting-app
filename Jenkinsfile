pipeline {
    agent {label 'app'}

    environment {
        AWS_ACCOUNT_ID = '974328657523'
        AWS_REGION = 'us-east-1'
        ECR_REPO = 'node-app-repo'
        CONTAINER_NAME = 'node-app'
        
    }

    stages {
        stage('Checkout Repository') {
            steps {
                git url: "https://github.com/shreyachatterjee1418/example-voting-app.git", branch: 'main'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                sh '''
                     cd vote
                     aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                     docker build -t $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:vote-v${BUILD_NUMBER} .
                     docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:vote-v${BUILD_NUMBER}
                '''
                
            }
        }
        stage('Deploy') {
            steps {
                    sh '''
                    docker pull $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:vote-v${BUILD_NUMBER}
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                    docker run -d --name $CONTAINER_NAME -p 80:8080 $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:vote-v${BUILD_NUMBER}
                    echo "Deployment successful. Application is running on port 80."
                    '''
                }
            }
    }
}

