pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Security Audit') {
            steps {
                sh 'npm audit --audit-level=high || true'
            }
        }

        stage('Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
