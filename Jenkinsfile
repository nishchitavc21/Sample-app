pipeline {
    agent any

    environment {
      IMAGE_NAME = "nishchitavc/sample-app"
      IMAGE_TAG = "${BUILD_NUMBER}"
    }


    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/nishchitavc21/Sample-app.git'
            }
        }

        

        stage('Build Docker Image'){
            steps{
                dir('Sample-app') {
                    sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
                }
            }
        }


        stage('Push Image') {
          steps {
            dir('Sample-app') {
              withCredentials([
                  usernamePassword(
                    credentialsId: 'DockerHub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                  )
                ])
                {
                  sh '''
                  echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                  docker push $IMAGE_NAME:$IMAGE_TAG
                  '''
                }  
            }
            
          }
        }

        stage('Deploy'){
          steps {
            dir('june-batch-cicd') {
                sh '''
                docker stop react-app || true
                docker rm react-app || true

                docker run -d --name react-app -p 80:80 $IMAGE_NAME:$IMAGE_TAG
                '''
            }
          }
        }
    }
}
