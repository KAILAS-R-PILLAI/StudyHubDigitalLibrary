pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '20'))
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install --cache .npm-cache'
            }
        }

        stage('Build') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'dev') {
                        echo 'Building DEV branch'
                        sh 'npm run build:dev || true'
                    } 
                    else if (env.BRANCH_NAME == 'stage') {
                        echo 'Building STAGE branch'
                        sh 'npm run build:stage || true'
                    } 
                    else if (env.BRANCH_NAME == 'prod') {
                        echo 'Building PROD branch'
                        sh 'npm run build:prod || true'
                    } 
                    else {
                        echo "Unknown branch: ${env.BRANCH_NAME}"
                    }
                }
            }
        }

        stage('Test') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'dev') {
                        echo 'Deploying to DEV environment'
                    } 
                    else if (env.BRANCH_NAME == 'stage') {
                        echo 'Deploying to STAGE environment'
                    } 
                    else if (env.BRANCH_NAME == 'prod') {
                        echo 'Deploying to PROD environment'
                    }
                }
            }
        }
    }

    post {
        success {
            emailext(
                to: 'kailasrpillai2074@gmail.com',
                subject: "Build SUCCESS: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                body: "Build succeeded! Check details at ${env.BUILD_URL}"
            )
        }

        failure {
            emailext(
                to: 'kailasrpillai2074@gmail.com',
                subject: "Build FAILED: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                body: "Build failed! Check console output at ${env.BUILD_URL}"
            )
        }
    }
}
