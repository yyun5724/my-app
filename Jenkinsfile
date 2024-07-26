pipeline {
    agent any

    environment {
        GIT_TIMEOUT = '120' // 增加超时时间到120秒
        DEPLOY_SERVER = '10.220.180.150' // 远程服务器地址
        DEPLOY_USER = 'root' // 远程服务器用户名
        DEPLOY_PASSWORD = 'Yinshuai+001' // 远程服务器密码
        DEPLOY_DIR = '/root/jenkins/' // 远程服务器部署目录
        JAR_FILE = 'target/my-app-1.0-SNAPSHOT.jar' // 打包后的 JAR 文件路径
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

        stage('Deploy') {
            steps {
                script {
                    def remote = [:]
                    remote.name = 'root'
                    remote.host = env.DEPLOY_SERVER
                    remote.user = env.DEPLOY_USER
                    remote.password = env.DEPLOY_PASSWORD
                    remote.allowAnyHosts = true

                    // 将 JAR 文件复制到远程服务器
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'my-remote-server',
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: env.JAR_FILE,
                                        removePrefix: '',
                                        remoteDirectory: env.DEPLOY_DIR,
                                        execCommand: "nohup java -jar ${env.DEPLOY_DIR}${env.JAR_FILE.split('/').last()} > ${env.DEPLOY_DIR}app.log 2>&1 &"
                                    )
                                ],
                                usePromotionTimestamp: false,
                                verbose: true
                            )
                        ]
                    )
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
