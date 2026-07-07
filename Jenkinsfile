pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-2'
        IMAGE_NAME = 'test-flask'
        REPO_NAME = 'test'
    }
    stages{
        stage('checkout'){
            steps{
                git 'https://github.com/jbrooks0929/testingjenkins'
        }
    }
        stage('Tag image'){
            steps{
                script(
                    IMAGE_TAG = 'latest'
                    )
            }
        }
        stage('Login in ECR'){
            steps{
                withAWS(region: "${env.AWS_REGION}", credentials: 'aws-creds'){
                        powershell '''
                        $ecrLogin = aws ecr get-login-password --region $env.AWS_REGION

                        docker login --username AWS --password $ecrLogin https://676327216025.dkr.ecr.us-east-2.amazonaws.com/testaws
                        '''
                        }
            }
        stage('Build Docker Image'){
            steps{
                powershell '''
                docker built -t $env.IMAGE_NAME:$env.IMAGE_TAG .
                docker tag $env.IMAGE_NAME:$env.IMAGE_TAG 676327216025.dkr.ecr.us-east-2.amazonaws.com/testaws:latest
                '''
            }
        }
        stage('Pushing to ECR'){
        steps{
            powershell '''
            docker push 676327216025.dkr.ecr.us-east-2.amazonaws.com/testaws:latest
            '''
        }
        }
        }
}

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jbrooks0929/testingjenkins'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:latest .'
            }
        }

        stage('Push DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat '''
docker login -u %DOCKER_USER% -p %DOCKER_PASS%
docker push %IMAGE_NAME%:latest
docker logout
'''
                }
            }
        }

        stage('Deploy') {
            steps {
                bat 'echo Deploying application...'
            }
        }
    }
}
