pipeline {
    agent any

    environment {
        APP_NAME = 'projectA'
        REGISTRY = 'localhost:32000'
        K8S_DIR = 'k8s'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                script {
                    // Use BUILD_NUMBER provided by Jenkins
                    sh """
                        docker build --no-cache -t ${REGISTRY}/${APP_NAME}:${BUILD_NUMBER} .
                        docker push ${REGISTRY}/${APP_NAME}:${BUILD_NUMBER}
                    """
                }
            }
        }

        stage('Update Deployment & Apply') {
            steps {
                script {
                    sh """
                        sed -i "s|image: ${REGISTRY}/${APP_NAME}:.*|image: ${REGISTRY}/${APP_NAME}:${BUILD_NUMBER}|" ${K8S_DIR}/deployment.yaml
                        kubectl apply -f ${K8S_DIR}/deployment.yaml
                        kubectl apply -f ${K8S_DIR}/service.yaml
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Build finished.'
        }
    }
}