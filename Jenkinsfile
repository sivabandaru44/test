pipeline {
    agent any

    environment {
        AWS_REGION = 'eu-north-1'
        IMAGE_NAME = 'test'
        REPO_NAME = 'project'
        IMAGE_TAG = 'latest'
        ECR_URL = '991169314132.dkr.ecr.eu-north-1.amazonaws.com'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sivabandaru44/test'
            }
        }

        stage('Login to ECR') {
            steps {
                withAWS(region: "${env.AWS_REGION}", credentials: 'aws-cred') {
                    powershell """
                    aws ecr get-login-password --region ${env.AWS_REGION} | docker login --username AWS --password-stdin ${env.ECR_URL}
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                powershell """
                docker build -t ${env.IMAGE_NAME}:${env.IMAGE_TAG} .
                docker tag ${env.IMAGE_NAME}:${env.IMAGE_TAG} ${env.ECR_URL}/${env.REPO_NAME}:${env.IMAGE_TAG}
                """
            }
        }

        stage('Push to ECR') {
            steps {
                powershell """
                docker push ${env.ECR_URL}/${env.REPO_NAME}:${env.IMAGE_TAG}
                """
            }
        }
    }
}
