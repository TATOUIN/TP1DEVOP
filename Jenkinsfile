pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "tatouin/devops-app"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/TATOUIN/TP1DEVOP.git'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'pip install -r requirements.txt'
                sh 'python -m unittest discover tests'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment/deployment.yaml'
                sh 'kubectl apply -f deployment/service.yaml'
            }
        }
    }
}
