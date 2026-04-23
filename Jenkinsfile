pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code already loaded from SCM'
            }
        }

        stage('Compile') {
            steps {
                sh 'javac src/HelloWorld.java'
            }
        }

        stage('Run') {
            steps {
                sh 'java -cp src HelloWorld'
            }
        }
    }
}
