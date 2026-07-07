pipeline {
    agent any

    environment {
        IMAGE_NAME = 'jbrooks0929/testingjenkins'
    }

    stages {

        stage('Check Environment') {
            steps {
                bat 'echo PATH=%PATH%'
                bat 'where docker'
                bat 'docker --version'
            }
        }

        stage('Push DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker',
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
