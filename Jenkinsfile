
pipeline {
    agent any

    tools {
        sonarRunner 'SonarScanner'
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
                echo "==== Files in workspace ===="
                ls -l
                '''
            }
        }

        stage('SonarQube Scan') {
            steps {
                sh '''
                sonar-scanner \
                  -Dsonar.projectKey=jenkins \
                  -Dsonar.projectName=jenkins \
                  -Dsonar.sources=. \
                  -Dsonar.host.url=http://localhost:9000 \
                  -Dsonar.login=sqp_464e44936e377a2a1d6e2f18d94498cd2eb1cb7d
                '''
            }
        }
    }

    post {
        success {
            echo '✅ SonarQube scan completed successfully'
        }
        failure {
            echo '❌ Pipeline failed'
        }
    }
}
