pipeline {
    agent any

    environment {
        IMAGE_NAME = "<dockerhub-username>/hello-java-app:v1"
    }

    stages {
        stage('Compile') {
            steps {
                sh 'javac src/HelloWorld.java'
            }
        }

        stage('Run Java') {
            steps {
                sh 'java -cp src HelloWorld'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run --rm $IMAGE_NAME'
            }
        }

        stage('Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
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
