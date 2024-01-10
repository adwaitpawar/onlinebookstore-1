pipeline {
    agent any
    
    environment {
        ENV_NAME = 'unknown'
    }

    tools {
        maven "MAVEN_HOME"
    }

    stages {
        stage('Checkout from Git') {
            steps {
                script {
                    checkout(
                        scm: [$class: 'GitSCM', branches: [[name: "*/${env.BRANCH_NAME}"]], userRemoteConfigs: [[url: 'https://github.com/adwaitpawar/onlinebookstore-1.git']]]
                    )

                    if (env.BRANCH_NAME == 'J2EE') {
                        ENV_NAME = 'production'
                    } else if (env.BRANCH_NAME == 'dev') {
                        ENV_NAME = 'development'
                    }
                }
            }
        }

        stage('Build') {
            when {
                expression { env.BRANCH_NAME == 'J2EE' || env.BRANCH_NAME == 'dev' }
            }
            steps {
                script {
                    bat 'mvn clean package'
                }
            }
        }

        stage('Deploy') {
            when {
                expression { env.BRANCH_NAME == 'J2EE' }
            }
            steps {
                script {
                    echo "Deploying to ${ENV_NAME} environment"
                    deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://localhost:8045/manager/html')],
                           contextPath: 'onlinebook1',
                           war: '**/*.war'
                }
            }
        }
    }
}
