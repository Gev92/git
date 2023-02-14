pipeline {
    agent any
    
    environment {
        DOCKER_USER = credentials('DOCKER_USER')
        DOCKER_PASSWORD = credentials('DOCKER_PASSWORD')
        AWS_CREDENTIALS = credentials('AWS_CREDENTIALS')

    }

    stages {
        stage('build') {
            steps {
                script {
                    def costumImage = docker.build("${DOCKER_USER}/my-image:${env.BUILD_ID}", "-f Dockerfile .")
                }
            }
        }

        stage('login to dockerhub') {
            steps {
                script {
                    sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USER} --password-stdin"
                }
            }
        }

        stage('push to dockerhub') {
            steps {
                script {
                    sh "docker push ${DOCKER_USER}/my-image:${env.BUILD_ID}"
                }
            }
        }

        stage('deploy to remote server') {
            steps {
                script {
                    // def remote = [:]
                    // remote.name = 'deploy1'
                    // remote.host = '35.174.211.107'
                    // remote.allowAnyHosts = true

                    withAWS(credentials:'AWS_CREDENTIALS', region: 'us_east_2') {
                        sh "kubectl apply -f " k8s/
                }

                    // withCredentials([sshUserPrivateKey(credentialsId: 'SSH_USER', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
                    // remote.user = userName
                    // remote.identityFile = identity
                    // sshCommand remote: remote, command: '''
                    //     if docker ps |grep 'nginx-app'; then
                    //         docker rm -f nginx-app
                    //     fi
                    // '''
                    // sshCommand remote: remote, command: "docker run -tid -p 80:80 --name nginx-app ${DOCKER_USER}/my-image:${env.BUILD_ID}"
                }
            }
        }
    }
}
}