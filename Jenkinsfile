pipeline {
    environment {
        DEPLOY = "${env.BRANCH_NAME == "main" || env.BRANCH_NAME.contains("develop") ? "true" : "false"}"
        // NAME = "${env.BRANCH_NAME == "main" ? "example" : "example-staging"}"
        // VERSION = ${env.BUILD_ID}
        // DOMAIN = 'localhost'
        REGISTRY = 'swlidoc/tomcatsample'
        // REGISTRY_CREDENTIAL = 'dockerhub-davidcampos'
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
                container('docker') {
                    // sh "apt update && sudo apt upgrade && apt install curl"
                    // sh "curl -fsSL https://get.docker.com -o get-docker.sh"
                    sh "nerdctl --version"
                    sh "docker build -t ${REGISTRY}:${env.BUILD_ID} ."
                }
            }
        }
        // stage('Docker Publish') {
        //     when {
        //         environment name: 'DEPLOY', value: 'true'
        //     }
        //     steps {
        //         container('docker') {
        //             withDockerRegistry([credentialsId: "${REGISTRY_CREDENTIAL}", url: ""]) {
        //                 sh "docker push ${REGISTRY}:${VERSION}"
        //             }
        //         }
        //     }
        // }
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
