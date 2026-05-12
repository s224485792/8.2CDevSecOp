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

                    def testStatus = sh(
                        script: 'npm test',
                        returnStatus: true
                    )

                    emailext(
                        subject: testStatus == 0 ?
                            "✅ Tests Passed" :
                            "❌ Tests Failed",

                        body: testStatus == 0 ?
                            "The test stage completed successfully." :
                            "The test stage has failed. Please review the attached logs.",

                        to: "supergtrain@gmail.com",
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

                    def auditStatus = sh(
                        script: 'npm audit --audit-level=high',
                        returnStatus: true
                    )

                    emailext(
                        subject: auditStatus == 0 ?
                            "✅ Security Scan Passed" :
                            "⚠️ Security Vulnerabilities Found",

                        body: "Security scan finished with status code: ${auditStatus}. Logs attached.",

                        to: "supergtrain@gmail.com",
                        attachLog: true
                    )

                    if (auditStatus != 0) {
                        echo "Security vulnerabilities detected."
                    }
                }
            }
        }

        stage('Coverage Report') {
            steps {
                sh 'npm run coverage'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}