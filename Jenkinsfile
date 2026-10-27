pipeline {
    agent any
    stages {
        stage('Build & Test') {
            steps {
                bat 'mvn clean package'
            }
        }
        stage('Docker Build') {
            steps {
                bat 'docker build -t campus-equipment-booking:set-e .'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                bat 'minikube image load campus-equipment-booking:set-e'
                bat 'kubectl apply -f deployment.yaml'
                bat 'kubectl apply -f service.yaml'
                bat 'kubectl scale deployment campus-equipment-deployment --replicas=2'
            }
        }
    }
}