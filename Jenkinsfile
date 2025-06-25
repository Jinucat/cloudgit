pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // 코드 저장소에서 소스코드를 가져옵니다.
                git 'https://github.com/your-repo/example.git'
            }
        }

        stage('Build') {
            steps {
                // 빌드 명령어 실행 (예: Gradle, Maven, npm 등)
                sh './gradlew build' // gradle 예시
            }
        }

        stage('Test') {
            steps {
                // 테스트 명령어 실행 (예: junit, mocha, pytest 등)
                sh './gradlew test' // gradle 예시
            }
            post {
                always {
                    // 테스트 결과 리포트(junit 등) 게시
                    junit 'build/test-results/test/*.xml'
                }
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                // 배포 또는 아티팩트 업로드 스크립트
                echo 'Deploying application...'
            }
        }
    }
    post {
        always {
            // 빌드 후 정리작업 등
            cleanWs()
        }
    }
}
