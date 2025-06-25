pipeline {
    agent any

    environment {
        IMAGE_NAME  = "docker.io/yourdockerid/myapp"
        IMAGE_TAG   = "3"  // 또는 "${BUILD_NUMBER}"로 자동화 가능
        DEPLOY_FILE = "k8s/deployment.yaml"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-org/your-repo.git'
            }
        }

        stage('Update Deployment File') {
            steps {
                // 이미지 태그를 yaml에 반영
                sh """
                  sed -i 's|image: .*|image: ${IMAGE_NAME}:${IMAGE_TAG}|' ${DEPLOY_FILE}
                """
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
