pipeline {
    agent any

    environment {
        IMAGE_NAME = 'jbrooks0929/testingjenkins'
    }

    stages {

        stage('Check Environment') {
            steps {
                git branch: 'main', url: 'https://github.com/jbrooks0929/testingjenkins'
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
