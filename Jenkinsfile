pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Helm Lint') {
            steps {
                sh "helm lint ./charts/nginx-custom"
            }
        }

        stage('Deploy to Minikube') {
            steps {
                withCredentials([file(credentialsId: 'minikube-kubeconfig', variable: 'KUBECONFIG')]) {
                    sh """
                    helm upgrade --install lab-ubuntu-nginx ./charts/nginx_lb \
                        --namespace default \
                        --wait
                    """
                }
            }
        }
    }
}
