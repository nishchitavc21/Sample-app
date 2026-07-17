pipeline {
    agent any

    environment {
        IMAGE_NAME = "nishchitavc/sample-app"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                // Use Jenkins env interpolation for IMAGE_NAME
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
                    // Windows batch uses %VAR% to access env vars
                    bat """
                    echo USER=%DOCKER_USER%
                    echo PASS-LEN=%DOCKER_PASS:~0,3%***
                    """
                }
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'Dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    // Pipe the actual password variable to docker login
                    bat """
                    echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                // Use Jenkins env interpolation for IMAGE_NAME here too
                bat "docker push ${env.IMAGE_NAME}"
            }
        }
    }
}