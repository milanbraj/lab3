pipeline {
    agent any

    environment {
        LOCAL_CHART_PATH = "/charts/nginx-custom" 
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Helm Lint') {
            steps {
                sh """
                ls -R
                helm lint ${LOCAL_CHART_PATH}
                
                """
            }
        }

        stage('Deploy to Minikube') {
            steps {
                withCredentials([file(credentialsId: 'minikube-kubeconfig', variable: 'KUBECONFIG')]) {
                    sh """
                    helm upgrade --install lab-ubuntu-nginx ${LOCAL_CHART_PATH} \
                        --namespace default \
                        --wait
                    """
                }
            }
        }
    }
}
