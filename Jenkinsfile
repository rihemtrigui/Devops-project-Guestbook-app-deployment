pipeline {
    agent any

    environment {
        REGISTRY = 'us.icr.io/sn-labs-rihem'
        IMAGE_NAME = 'guestbook'
        TAG = 'v1'
        DOCKER_CREDENTIALS_ID = 'myregistrykey'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from your repository
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${REGISTRY}/${IMAGE_NAME}:${TAG}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry("https://${REGISTRY}", "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${REGISTRY}/${IMAGE_NAME}:${TAG}").push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Apply Kubernetes deployment using the existing deployment YAML
                sh 'kubectl apply -f deployment.yml'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
