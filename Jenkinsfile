pipeline {
    agent any

    environment {
        REGISTRY    = "docker.io/yourdockerid"
        IMAGE_NAME  = "myapp"
        IMAGE_TAG   = "${BUILD_NUMBER}"
        KUBE_DEPLOY_PATH = "k8s/deployment.yaml"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-org/your-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $REGISTRY/$IMAGE_NAME:$IMAGE_TAG ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login -u yourdockerid --password-stdin"
                }
                sh "docker push $REGISTRY/$IMAGE_NAME:$IMAGE_TAG"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // 이미지 태그를 배포 yaml에 반영 (단순 치환 예시)
                sh "sed -i 's|image: .*\$|image: ${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}|' ${KUBE_DEPLOY_PATH}"

                // 실제 배포
                withKubeConfig([credentialsId: 'kubeconfig-jenkins']) {
                    sh "kubectl apply -f $KUBE_DEPLOY_PATH"
                }
            }
        }
    }
}
