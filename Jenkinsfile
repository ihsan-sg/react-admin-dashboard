pipeline {
    agent any

    environment {
        NODE_VERSION = '22.14.0-alpine3.21'
    }

    parameters {
        choice(name: 'ENVIRONMENT', choices: ['development', 'staging', 'production'], description: 'Select the deployment environment')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/ihsan-sg/react-admin-dashboard.git'
            }
        }

        stage('Set Environment Variables') {
            steps {
                script {
                    if (params.ENVIRONMENT == 'development') {
                        env.API_URL = 'https://dev.api.example.com'
                        env.DB_CONNECTION = 'dev-db'
                    } else if (params.ENVIRONMENT == 'staging') {
                        env.API_URL = 'https://staging.api.example.com'
                        env.DB_CONNECTION = 'staging-db'
                    } else if (params.ENVIRONMENT == 'production') {
                        env.API_URL = 'https://api.example.com'
                        env.DB_CONNECTION = 'prod-db'
                    }
                    echo "Selected Environment: ${params.ENVIRONMENT}"
                    echo "API URL: ${env.API_URL}"
                    echo "Database Connection: ${env.DB_CONNECTION}"
                }
            }
        }

        stage('Build') {
            agent {
                docker {
                    image "node:${NODE_VERSION}"
                    args '-w /app'
                }
            }
            steps {
                script {
                    sh 'npm install'
                    sh 'npm run build'
        
