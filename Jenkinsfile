pipeline {
    agent { label 'Agent-1' }

    environment {
        PROJECT = 'EXPENSE'
        COMPONENT = 'BACKEND'
        DEPLOY_TO = "production"
    }

    options {
        disableConcurrentBuilds()
        timeout(time: 30, unit: 'MINUTES')
    }

    stages {
        stage('Read version') {
            steps {
                script {
                    sh """
                        def packageJson = readJSON file: 'package.json'
                        appVersion = packageJson.version
                        echo "version is: $appVersion "
                    """
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh """
                        echo "Hello, this is test"
                    """
                }
            }
        }

        stage('Deploy') {
            when {
                environment name: 'DEPLOY_TO', value: 'production'
            }
            steps {
                script {
                    sh """
                        echo "Hello, this is deploy"
                    """
                }
            }
        }

        stage('Parallel Stages') {
            parallel {
                stage('STAGE-1') {
                    steps {
                        script {
                            sh """
                                echo "Hello, this is STAGE-1"
                                sleep 15
                            """
                        }
                    }
                }

                stage('STAGE-2') {
                    steps {
                        script {
                            sh """
                                echo "Hello, this is STAGE-2"
                                sleep 15
                            """
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'I will always say Hello again!'
            deleteDir()
        }
        failure {
            echo 'I will run when pipeline is failed'
        }
        success {
            echo 'I will run when pipeline is success'
        }
    }
}