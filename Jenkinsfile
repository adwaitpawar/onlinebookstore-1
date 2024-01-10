pipeline {
    agent any
    
    tools {
        maven "MAVEN_HOME"
    }
    
    stages {
        stage('Checkout from Git') {
            steps {
                script {
                    checkout(
                        branches: [[name: '*/J2EE']],
                        scm: [$class: 'GitSCM', branches: [[name: '*/dev']], userRemoteConfigs: [[url: 'https://github.com/adwaitpawar/onlinebookstore-1.git']]]
                    )
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
                    echo 'Deploying to Tomcat server'
                    deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://localhost:8045/manager/html')],
                           contextPath: 'onlinebookstore-dev',
                           war: '**/*.war'
                }
            }
        }
    }
}
