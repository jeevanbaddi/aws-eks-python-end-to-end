pipeline {
  agent any

  environment {
    AWS_REGION     = "ap-south-1"
    AWS_ACCOUNT_ID = "176713589590"
    ECR_REPO       = "python-eks-app"
    IMAGE_NAME     = "python-app"
    IMAGE_TAG      = "latest"
  }

  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
          docker build -t $IMAGE_NAME:$IMAGE_TAG .
        '''
      }
    }

    stage('Login to AWS ECR') {
      steps {
        sh '''
          aws ecr get-login-password --region $AWS_REGION \
          | docker login --username AWS --password-stdin \
          $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
        '''
      }
    }

    stage('Tag Image for ECR') {
      steps {
        sh '''
          docker tag $IMAGE_NAME:$IMAGE_TAG \
          $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:$IMAGE_TAG
        '''
      }
    }

    stage('Push Image to ECR') {
      steps {
        sh '''
          docker push \
          $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:$IMAGE_TAG
        '''
      }
    }
  }

  post {
    always {
      sh 'docker logout || true'
    }
  }
}
