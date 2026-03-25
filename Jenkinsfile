pipeline {
    agent { label 'Agent-1' }

    environment {
        PROJECT   = 'EXPENSE'
        COMPONENT = 'BACKEND'
        DEPLOY_TO = 'production'
        ACC_ID    = '625981588289'
    }

    options {
        disableConcurrentBuilds()
        timeout(time: 30, unit: 'MINUTES')
    }

    stages {

        stage('Read version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    def appVersion = packageJson.version
                    echo "Version is: ${appVersion}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Docker Login (ECR)') {
            steps {
                withAWS(region: 'ap-south-1', credentials: 'aws-creds') {
                    sh """
                        aws ecr get-login-password --region ap-south-1 | \
                        docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.ap-south-1.amazonaws.com
                    """
                }
            }
        }

        stage('Parallel Stages') {
            parallel {

                stage('STAGE-1') {
                    steps {
                        sh """
                            echo "Hello, this is STAGE-1"
                            sleep 15
                        """
                    }
                }

                stage('STAGE-2') {
                    steps {
                        sh """
                            echo "Hello, this is STAGE-2"
                            sleep 15
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'I will always say Hiello again!'
            deleteDir()
        }
        failure {
            echo 'Pipeline failed'
        }
        success {
            echo 'Pipeline succeeded'
        }
    }
}