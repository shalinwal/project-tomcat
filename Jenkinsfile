pipeline {
    environment {
        DEPLOY = "${env.BRANCH_NAME == "main" || env.BRANCH_NAME.contains("features") ? "true" : "false" || env.BRANCH_NAME.contains("develop") ? "true" : "false"}"
        // NAME = "${env.BRANCH_NAME == "main" ? "example" : "example-staging"}"
        // VERSION = ${env.BUILD_ID}
        // DOMAIN = 'localhost'
        REGISTRY = 'swlidoc/tomcatsample'
        REGISTRY_CREDENTIAL = 'dockerhub-shalini'
    }
    agent {
        kubernetes {
            defaultContainer 'jnlp'
            yamlFile 'build.yaml'
        }
    }
    stages {
        // stage('Build') {
        //     steps {
        //         container('docker') {
        //             sh "docker --version"
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
                        "apt update && apt upgrade -y && apt install curl -y && apt install sudo -y"
                        "curl -fsSL https://get.docker.com/ | sh"
                        // sh "ulimit -n 10240"
                        "sudo service docker stop"
                        "echo \"limit nofile 262144 262144\" >> /etc/init/docker.conf"
                        "sudo service docker start"
                        // sh "sudo dockerd"
                        "sudo chmod 666 /var/run/docker.sock"
                        "sleep 10"
                        "docker --version"
                        // "docker build -t ${REGISTRY}:${env.EXECUTOR_NUMBER} ."
                        dockerImage = docker.build ${REGISTRY} + ":$BUILD_NUMBER"
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
                    container('docker') {
                        docker.withRegistry('', "${REGISTRY_CREDENTIAL}") {
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
