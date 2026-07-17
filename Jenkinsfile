pipeline {
    agent any

    environment {
        IMAGE_NAME = "nishchitavc/sample-app"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                bat "docker build -t ${env.IMAGE_NAME} ."
            }
        }

        stage('Debug Docker Creds') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'Dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat """
                    echo USER=%DOCKER_USER%
                    echo PASS-LEN=%DOCKER_PASS:~0,3%***
                    """
                }
            }
        }

        stage('Docker Login (debug)') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'Dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat """
                    docker logout
                    docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat "docker push ${env.IMAGE_NAME}"
            }
        }
    }
}