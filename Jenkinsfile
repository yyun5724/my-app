pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git(
                    url: 'https://github.com/yyun5724/my-app.git',
                    branch: 'main
                )
            }
        }
        
        stage('Build') {
            steps {
                script {
                    def mvnHome = tool name: 'Maven 3.6.3', type: 'Maven'
                    env.PATH = "${mvnHome}/bin:${env.PATH}"
                }
                sh 'mvn clean package'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml' // 确保测试报告路径正确
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }
}

