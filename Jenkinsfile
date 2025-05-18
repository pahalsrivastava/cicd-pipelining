pipeline {
    agent any

    stages {
        stage('Pull Docker Images') {
            steps {
                bat 'docker pull rishamk/motobikes-frontend:latest'
                bat 'docker pull rishamk/motobikes-backend:latest'
            }
        }

        stage('Stop Old Containers (if running)') {
            steps {
                // To avoid failure if container doesn't exist, ignore errors
                bat 'docker rm -f frontend || exit 0'
                bat 'docker rm -f backend || exit 0'
            }
        }

        stage('Run Frontend & Backend Containers') {
            steps {
                bat 'docker run -d --name frontend -p 30323:80 rishamk/motobikes-frontend:latest'
                bat 'docker run -d --name backend -p 32535:5555 rishamk/motobikes-backend:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfignew-file', variable: 'KUBECONFIG_FILE')]) {
                    bat """
                    set KUBECONFIG=%KUBECONFIG_FILE%
                    kubectl apply -f k8s/backend-deployment.yaml
                    kubectl apply -f k8s/frontend-deployment.yaml
                    kubectl apply -f k8s/backend-service.yaml
                    kubectl apply -f k8s/frontend-service.yaml
                    """
                }
            }
        }
    }
}
