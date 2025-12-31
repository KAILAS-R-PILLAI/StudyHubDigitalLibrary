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
                echo 'Building MAIN branch'
                sh 'npm run build || echo "No build step for main"'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to MAIN environment'
            }
        }
    }

    post {
        success {
            emailext(
                to: 'kailasrpillai2074@gmail.com',
                subject: "MAIN Build SUCCESS: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                body: "MAIN Build succeeded! Check details at ${env.BUILD_URL}"
            )
        }

        failure {
            emailext(
                to: 'kailasrpillai2074@gmail.com',
                subject: "MAIN Build FAILED: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                body: "MAIN Build failed! Check console output at ${env.BUILD_URL}"
            )
        }
    }
}

