pipeline {
    environment {
        DEPLOY = "${env.BRANCH_NAME == "main" || env.BRANCH_NAME.contains("features") ? "true" : "false" || env.BRANCH_NAME.contains("develop") ? "true" : "false"}"
        // NAME = "${env.BRANCH_NAME == "main" ? "example" : "example-staging"}"
        HELM_RELEASE = "tomcat-deployment"
        // VERSION = ${env.BUILD_ID}
        // DOMAIN = 'localhost'
        REGISTRY = "swlidoc/tomcatsample"
        REGISTRY_CREDENTIAL = 'dockerhub-shalini'
        IMAGEPULL_SECRET = 'dockersecret'
        // dockerImage = ''
        dockerImage = 'swlidoc/tomcatsample:d22067746f6684c57bd55d689c0891c5d9d22652'
        // MY_ID = $("${env.BRANCH_NAME}-${currentBuild.id}" | tr -dc [A-Za-z0-9-])
        //MY_ID = "${env.BRANCH_NAME}-${currentBuild.id}" | tr -dc '[:alnum:]\n\r' | tr '[:upper:]' '[:lower:]'
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
        // stage('Docker Build') {
        //     when {
        //         environment name: 'DEPLOY', value: 'true'
        //     }
        //     steps {
        //         script {
        //             container('ubuntu') {
        //                 sh "apt update && apt upgrade -y && apt install curl -y && apt install sudo -y"
        //                 sh "curl -fsSL https://get.docker.com/ | sh"
        //                 // sh "ulimit -n 10240"
        //                 sh "sudo service docker stop"
        //                 sh "echo \"limit nofile 262144 262144\" >> /etc/init/docker.conf"
        //                 sh "sudo service docker start"
        //                 // sh "sudo dockerd"
        //                 sh "sudo chmod 666 /var/run/docker.sock"
        //                 sh "sleep 10"
        //                 sh "docker --version"
        //                 // "docker build -t ${REGISTRY}:${env.EXECUTOR_NUMBER} ."
        //                 dockerImage = docker.build REGISTRY + ":$GIT_COMMIT"
        //             }
        //         }
        //     }
        // }
        // stage('Docker Publish') {
        //     when {
        //         environment name: 'DEPLOY', value: 'true'
        //     }
        //     steps {
        //         script {
        //             container('ubuntu') {
        //                 docker.withRegistry('', REGISTRY_CREDENTIAL) {
        //                     dockerImage.push()
        //                 }
        //             }
        //         }
        //     }
        // }
        stage('Kubernetes Deploy') {
            when {
                environment name: 'DEPLOY', value: 'true'
            }
            steps {
                container('helm') {
                    // sh "apt update && apt upgrade -y && apt install curl -y && apt install sudo -y"
                    // sh "curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3"
                    // sh "sudo chmod 700 get_helm.sh"
                    // sh "./get_helm.sh"
                    sh "helm upgrade --install --set deployment.image=dockerImage --set secret.securestring=IMAGEPULL_SECRET ${HELM_RELEASE} ./helm-deployment"
                }
            }
        }
    }
}
