pipeline {

    agent any

    environment {

        IMAGE_NAME = "nishchitavc/sample-app"

    }

    stages {

        stage('Clone Repository') {

            steps {

                git 'https://github.com/nishchitavc21/Sample-app'

            }

        }

        stage('Build Docker Image') {

            steps {

                sh 'docker build -t $IMAGE_NAME .'

            }

        }

        stage('Docker Login') {

            steps {

                withCredentials([usernamePassword(credentialsId: 'DockerHub',

                usernameVariable: 'USER',

                passwordVariable: 'PASS')]) {

                    sh 'echo $PASS | docker login -u $USER --password-stdin'

                }

            }

        }

        stage('Push Docker Image') {

            steps {

                sh 'docker push $IMAGE_NAME'

            }

        }

    }

}