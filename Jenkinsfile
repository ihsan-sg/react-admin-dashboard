/* Requires the Docker Pipeline plugin */
pipeline {
    agent { docker { image 'node:22.14.0-alpine3.21' } }

    parameters {
        choice(name: 'ENV', choices: ['development', 'testing', 'production'], description: 'Select the environment')
    }

    environment {
        NODE_VERSION = '22.14.0'
    }

    stages {
        stage('Setup') {
            steps {
                script {
                    if (params.ENV == 'development') {
                        echo "Running in Development Environment"
                        env.CONFIG_FILE = 'config/dev-config.yml'
                    } else if (params.ENV == 'testing') {
                        echo "Running in Testing Environment"
                        env.CONFIG_FILE = 'config/test-config.yml'
                    } else if (params.ENV == 'production') {
                        echo "Running in Production Environment"
                        env.CONFIG_FILE = 'config/prod-config.yml'
                    }
                }
            }
        }

        stage('Build') {
            steps {
                sh 'node --version'
                echo "Using configuration file: ${env.CONFIG_FILE}"
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (params.ENV == 'production') {
                        input message: 'Approve Production Deployment?'
                    }
                    echo "Deploying to ${params.ENV} environment..."
                    sh "deploy.sh --env ${params.ENV} --config ${env.CONFIG_FILE}"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline execution completed for ${params.ENV}"
        }
    }
}
