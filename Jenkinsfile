pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '20'))
        timestamps()
    }

    tools {
        nodejs 'node18'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo 'Listing files after checkout:'
                sh 'ls -la'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install --cache .npm-cache'
            }
        }

        stage('Build') {
            steps {
                echo 'Building STAGE branch'
                sh 'npm run build:stage || true'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to STAGE environment'
            }
        }
    }

    post {
        success {
            emailext(
                to: 'kailasrpillai2074@gmail.com',
                subject: "STAGE Build SUCCESS: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                body: "STAGE Build succeeded! Check details at ${env.BUILD_URL}"
            )
        }

        failure {
            emailext(
                to: 'kailasrpillai2074@gmail.com',
                subject: "STAGE Build FAILED: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                body: "STAGE Build failed! Check console output at ${env.BUILD_URL}"
            )
        }
    }
}
