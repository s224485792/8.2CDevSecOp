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
                script {
                    
                    def testStatus = sh(script: 'npm test', returnStatus: true)

                    
                    emailext(
                        subject: testStatus == 0 ? " TESTS PASSED" : " TESTS FAILED",
                        body: testStatus == 0 ?
                            "The test stage completed successfully." :
                            "The test stage has failed. Please review the attached logs.",
                        to: "s224485792@deakin.edu.au",
                        attachLog: true
                    )

                    
                    if (testStatus != 0) {
                        error("Tests failed")
                    }
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    
                    def auditStatus = sh(script: 'npm audit --audit-level=high || true', returnStatus: true)

                    
                    emailext(
                        subject: " Security Scan Completed",
                        body: "Security scan finished with status code: ${auditStatus}. Logs attached.",
                        to: "s224485792@deakin.edu.au",
                        attachLog: true
                    )
                }
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
