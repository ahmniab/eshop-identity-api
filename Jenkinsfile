pipeline {
    agent any

    environment {
        IMAGE_NAME = "ahmniab"
        TAG = "1.0.0"
    }

    stages {
//         stage('list dir') {
//             steps {
//                 sh """
//                     -v ${WORKSPACE}:/app \
//                     -w /app \
//                     mcr.microsoft.com/dotnet/sdk:10.0 \
//                     ls
//                 """
//             }
//         }
        stage('Run Tests') {
            steps {
                sh """
                docker \
                    -v ${WORKSPACE}:/app \
                    -w /app \
                    mcr.microsoft.com/dotnet/sdk:10.0 \
                    dotnet test eShop.Identity.API.sln
                """
            }

        }
        stage('Login to docker') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                    sh 'echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin'
                }
            }
        }
        stage('Build and Push Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
                sh 'docker push $IMAGE_NAME:$TAG'
            }
        }
    }
}
