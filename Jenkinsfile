pipeline {
    agent any

    environment {
        DEPLOY_SERVER = '10.220.180.150'
        DEPLOY_DIR = '/root/jenkins/'
        JAR_FILE = 'target/your-package.jar'
        SSH_CREDENTIALS_ID = '10.220.180.150' // 使用刚刚创建的凭据ID
    }

    stages {
        stage('Deploy') {
            steps {
                script {
                    def remote = [:]
                    remote.name = 'my-remote-server'
                    remote.host = env.DEPLOY_SERVER
                    remote.user = 'your_username' // 根据你的凭据填写
                    remote.credentialsId = env.SSH_CREDENTIALS_ID
                    remote.allowAnyHosts = true

                    // 将 JAR 文件复制到远程服务器
                    sshPut remote: remote, from: env.JAR_FILE, into: env.DEPLOY_DIR

                    // 在远程服务器上执行启动命令
                    sshCommand remote: remote, command: "nohup java -jar ${env.DEPLOY_DIR}${env.JAR_FILE.split('/').last()} > ${env.DEPLOY_DIR}app.log 2>&1 &"
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
