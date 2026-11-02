
pipeline {
    agent any

    environment {
        IMAGE_NAME = "gauravchaudhari7165/hello-java-app:v2"
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
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                        set +x
                        echo "$DOCKER_PASS" |
                            docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }

        stage('Debug Kubernetes Connection') {
            steps {
                withCredentials([
                    file(
                        credentialsId: 'minikube-config-vm',
                        variable: 'KUBECONFIG'
                    )
                ]) {
                    sh '''
                        set -eu

                        echo "Checking Kubernetes context..."
                        kubectl config current-context

                        echo "Checking Kubernetes API..."
                        kubectl get --raw=/version

                        echo "Checking cluster nodes..."
                        kubectl get nodes
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([
                    file(
                        credentialsId: 'minikube-config-vm',
                        variable: 'KUBECONFIG'
                    )
                ]) {
                    sh '''
                        set -eu
                        kubectl apply -f deployment.yaml
                    '''
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                withCredentials([
                    file(
                        credentialsId: 'minikube-config-vm',
                        variable: 'KUBECONFIG'
                    )
                ]) {
                    sh '''
                        set -eu
                        kubectl get deployments
                        kubectl get pods
                        kubectl get services
                    '''
                }
            }
        }
    }
}
