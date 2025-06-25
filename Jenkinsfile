pipeline {
    agent any

    environment {
        IMAGE_NAME  = "docker.io/jinucat/cloudgit"
        IMAGE_TAG   = "latest"  // 또는 BUILD_NUMBER 등으로 교체 가능
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
                // image: ... 줄을 현재 태그로 치환
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
