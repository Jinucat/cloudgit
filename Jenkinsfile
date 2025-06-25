pipeline {
    agent any

    environment {
        IMAGE_NAME  = "docker.io/jinucat/cloudgit"
        IMAGE_TAG   = "latest"
        DEPLOY_FILE = "deployment.yaml"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Jinucat/cloudgit.git'
            }
        }

        stage('Update Deployment File') {
            steps {
                sh "sed -i 's|image: .*|image: ${IMAGE_NAME}:${IMAGE_TAG}|' ${DEPLOY_FILE}"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig-jenkins']) {
                    sh "kubectl apply -f ${DEPLOY_FILE}"
                }
            }
        }
    }
}
