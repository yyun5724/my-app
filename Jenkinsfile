pipeline {
    agent any

    environment {
        GIT_TIMEOUT = '120' // 增加超时时间到120秒
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'my-app', url: 'https://github.com/yyun5724/my-app.git', branch: 'main'
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
