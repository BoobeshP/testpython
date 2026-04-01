pipeline {
    agent any

    tools {
        sonarScanner 'SonarScanner'
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/BoobeshP/testpython.git', branch: 'main'
            }
        }

        stage('Show Files') {
            steps {
                sh '''
                echo "===== WORKSPACE FILES ====="
                ls -l
                '''
            }
        }

        stage('Run Python (optional)') {
            steps {
                sh '''
                echo "===== PYTHON VERSION ====="
                python3 --version || true
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                        sh '''
                        sonar-scanner \
                          -Dsonar.projectKey=jenkins \
                          -Dsonar.projectName=jenkins \
                          -Dsonar.sources=. \
                          -Dsonar.language=py \
                          -Dsonar.host.url=http://localhost:9000 \
                          -Dsonar.login=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo '✅ SonarQube analysis completed successfully'
        }
        failure {
            echo '❌ Pipeline failed'
        }
    }
}
``
