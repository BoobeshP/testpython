
pipeline {
    agent any

    stages {

        stage('Checkout Source Code') {
            steps {
                git url: 'https://github.com/BoobeshP/testpython.git', branch: 'main'
            }
        }

        stage('Show Workspace Files') {
            steps {
                sh '''
                echo "===== FILES IN WORKSPACE ====="
                ls -l
                '''
            }
        }

        stage('SonarQube Scan') {
            steps {
                sh '''
                echo "===== RUNNING SONARQUBE SCAN ====="

                /opt/sonar-scanner/bin/sonar-scanner \
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
            echo '✅ SonarQube analysis completed SUCCESSFULLY'
        }
        failure {
            echo '❌ SonarQube analysis FAILED'
        }
    }
}
}
