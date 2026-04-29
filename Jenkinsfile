pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-southeast-2'
        IMAGE_NAME = 'test'
        REPO_NAME = 'project'
        IMAGE_TAG = 'latest'
        ECR_URL = '377480205258.dkr.ecr.ap-southeast-2.amazonaws.com'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sivabandaru44/test'
            }
        }

        stage('Login to ECR') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWS_cred']]) {
                
                    bat '''
                    aws ecr get-login-password --region ap-southeast-2 ^
                    | docker login --username AWS --password-stdin 377480205258.dkr.ecr.ap-southeast-2.amazonaws.com
                    '''
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
