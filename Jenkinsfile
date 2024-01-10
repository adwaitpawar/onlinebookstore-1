pipeline {
    agent any
    
    environment {
        // Default value, in case the BRANCH_NAME environment variable is not available
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
                        branches: [[name: '*/J2EE']],
                        scm: [$class: 'GitSCM', branches: [[name: '*/J2EE']], userRemoteConfigs: [[url: 'https://github.com/adwaitpawar/onlinebookstore-1.git']]]
                    )

                    // Set environment variable based on branch name
                    if (env.BRANCH_NAME == 'J2EE') {
                        ENV_NAME = 'production'
                    } else if (env.BRANCH_NAME == 'dev') {
                        ENV_NAME = 'development'
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    bat 'mvn clean package'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying to ${ENV_NAME} environment"
                    deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://localhost:8045/manager/html')],
                           contextPath: 'onlinebookstore-1',
                           war: '**/*.war'
                }
            }
        }
    }
}
