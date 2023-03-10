pipeline {
    agent any
    
    environment {
        DOCKER_USER = credentials('DOCKER_USER')
        DOCKER_PASSWORD = credentials('DOCKER_PASSWORD')

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
                    withAWS(credentials:'AWS_CREDENTIALS', region: 'us-east-2') {
                        sh '''
                        aws eks --region us-east-2 update-kubeconfig --name K8s-exam
                        kubectl apply -f  k8s/
                        '''
                    }
                }
            }
        }
    }
}