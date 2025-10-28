pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = "harikagadagotti/flask-demo"
    }

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main', url: 'https://github.com/HarikaGadagotti/flask-demo'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_HUB_REPO:latest .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh 'docker push $DOCKER_HUB_REPO:latest'
                }
            }
        }
    }

    post {
        always {
            echo "Cleaning up Docker environment"
            sh 'docker logout'
        }
    }
}
