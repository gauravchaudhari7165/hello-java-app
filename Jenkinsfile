pipeline {
    agent any

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
                sh 'docker build -t hello-java-app:v1 .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run --rm hello-java-app:v1'
            }
        }
    }
}
