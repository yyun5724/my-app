pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/yyun5724/my-app.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    def mvnHome = tool name: 'Maven 3.6.3', type: 'maven'
                    sh "${mvnHome}/bin/mvn clean package"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    def mvnHome = tool name: 'Maven 3.6.3', type: 'maven'
                    sh "${mvnHome}/bin/mvn test"
                }
            }
        }
    }

    post {
        always {
            script {
                junit '**/target/surefire-reports/*.xml'
            }
        }
    }
}

