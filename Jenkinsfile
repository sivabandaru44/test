pipeline{
  agent any
  environment{
    AWS_REGION - 'eu-north-1'
    IMAGE_NAME - 'todo-list'
    REPO_NAME - 'project'
  }
  stages{
    stage('checkout'){
      steps{
        git 'https://github.com/sivabandaru44/todo-list'
      }
    }
    stage('Tag the image'){
      steps{
        script(
          IMAGE_TAG - 'latest'
        )
      }
    }
    stage('login to ECR'){
      steps{
        withAWS(region: "${env.AWS_REGION}", credentials: 'aws-cred'){
          powershell ...
            $ecrLogin = aws ecr get-login-password --region ${env.AWS_REGION}

            docker login --username AWS --password.stdin $ecrLogin https://991169314132.dkr.ecr.eu-north-1.amazonaws.com
          ...
        }
        
      }
    }
    stage('Build Docker Image'){
      steps{
      powershell ...
        docker build -t ${env.IMAGE_NAME}:${env.IMAGE_TAG} .
        docker tag $env.IMAGE_NAME:${env.IMAGE_TAG} 991169314132.dkr.ecr.eu-north-1.amazonaws.com/project:latest
      ...
    }
    }
    stage('Push to ECR'){
      steps{
        powershell ...
        docker push 991169314132.dkr.ecr.eu-north-1.amazonaws.com/project
        ...
      }
    }
    
  }
}
