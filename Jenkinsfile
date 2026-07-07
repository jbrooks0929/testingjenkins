pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-2'
        IMAGE_NAME = 'test-flask'
        REPO_NAME = 'test'
        IMAGE_TAG = 'latest'
        ECR_REPO = '676327216025.dkr.ecr.us-east-2.amazonaws.com/testaws'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/jbrooks0929/testingjenkins'
            }
        }

        stage('Login to ECR') {
            steps {
                powershell '''
$ecrLogin = aws ecr get-login-password --region $env:AWS_REGION
$ecrLogin | docker login --username AWS --password-stdin 676327216025.dkr.ecr.us-east-2.amazonaws.com
'''
            }
        }

        stage('Build Docker Image') {
            steps {
                powershell '''
docker build -t $env:IMAGE_NAME`:$env:IMAGE_TAG .
docker tag $env:IMAGE_NAME`:$env:IMAGE_TAG $env:ECR_REPO`:$env:IMAGE_TAG
'''
            }
        }

        stage('Push to ECR') {
            steps {
                powershell '''
docker push $env:ECR_REPO`:$env:IMAGE_TAG
'''
            }
        }
    }
}
