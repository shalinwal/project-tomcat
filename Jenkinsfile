pipeline {
    environment {
        DEPLOY = "${env.BRANCH_NAME == "main" || env.BRANCH_NAME.contains("features") ? "true" : "false" || env.BRANCH_NAME.contains("develop") ? "true" : "false"}"
        // NAME = "${env.BRANCH_NAME == "main" ? "example" : "example-staging"}"
        // VERSION = ${env.BUILD_ID}
        // DOMAIN = 'localhost'
        REGISTRY = "swlidoc/tomcatsample"
        REGISTRY_CREDENTIAL = 'dockerhub-shalini'
        dockerImage = ''
        MY_ID = echo ("${env.BRANCH_NAME}-${currentBuild.id}" | tr -dc [A-Za-z0-9-])
    }
    agent {
        kubernetes {
            defaultContainer 'jnlp'
            yamlFile 'build.yaml'
        }
    }
    stages {
        // stage('Test') {
        //     steps {
        //         container('ubuntu') {
        //             sh "apt update && apt upgrade -y && apt install curl -y"
        //             sh "kubectl config get-contexts"
        //         }
        //     }
        // }
        stage('Docker Build') {
            when {
                environment name: 'DEPLOY', value: 'true'
            }
            steps {
                script {
                    container('ubuntu') {
                        sh "apt update && apt upgrade -y && apt install curl -y && apt install sudo -y"
                        sh "curl -fsSL https://get.docker.com/ | sh"
                        // sh "ulimit -n 10240"
                        sh "sudo service docker stop"
                        sh "echo \"limit nofile 262144 262144\" >> /etc/init/docker.conf"
                        sh "sudo service docker start"
                        // sh "sudo dockerd"
                        sh "sudo chmod 666 /var/run/docker.sock"
                        sh "sleep 10"
                        sh "docker --version"
                        // "docker build -t ${REGISTRY}:${env.EXECUTOR_NUMBER} ."
                        dockerImage = docker.build REGISTRY + ":$MY_ID"
                    }
                }
            }
        }
        stage('Docker Publish') {
            when {
                environment name: 'DEPLOY', value: 'true'
            }
            steps {
                script {
                    container('ubuntu') {
                        docker.withRegistry('', REGISTRY_CREDENTIAL) {
                            dockerImage.push()
                        }
                    }
                }
            }
        }
        // stage('Kubernetes Deploy') {
        //     when {
        //         environment name: 'DEPLOY', value: 'true'
        //     }
        //     steps {
        //         container('helm') {
        //             sh "helm upgrade --install --force --set name=${NAME} --set image.tag=${VERSION} --set domain=${DOMAIN} ${NAME} ./helm"
        //         }
        //     }
        // }
    }
}
