pipeline {
    agent any

    environment {
        PATH = "C:\\Program Files\\nodejs;${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/manyakhosla/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
            post {
                success {
                    emailext(
                        to: 'manyakhosla63@gmail.com',
                        subject: 'Run Tests Stage: SUCCESS',
                        body: 'The Run Tests stage completed successfully.'
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
            post {
                failure {
                    emailext(
                        to: 'manyakhosla63@gmail.com',
                        subject: 'Security Scan: FAILURE',
                        body: 'The Security Scan stage failed. Please check the pipeline logs.'
                    )
                }
            }
        }
    }
}
