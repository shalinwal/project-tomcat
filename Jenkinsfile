pipeline {
    environment {
        DEPLOY = "${env.BRANCH_NAME == "main" || env.BRANCH_NAME.contains("features") ? "true" : "false" || env.BRANCH_NAME.contains("develop") ? "true" : "false"}"
        // NAME = "${env.BRANCH_NAME == "main" ? "example" : "example-staging"}"
        HELM_RELEASE = "tomcat-deployment"
        // VERSION = ${env.BUILD_ID}
        // DOMAIN = 'localhost'
        REGISTRY = "swlidoc/tomcatsample"
        REGISTRY_CREDENTIAL = 'dockerhub-push'
        IMAGEPULL_SECRET = credentials('dockersecret')
        dockerImage = ''
        //dockerImage = 'swlidoc/tomcatsample:d22067746f6684c57bd55d689c0891c5d9d22652'
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
        stage('Kubernetes Deploy') {
            when {
                environment name: 'DEPLOY', value: 'true'
            }
            steps {
                container('ubuntu') {
                    sh dockerimg = REGISTRY + ":$GIT_COMMIT"
                    sh "echo dockerimg"
                    // sh "apt update && apt upgrade -y && apt install curl -y && apt install sudo -y"
                    // // sh "curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -"
                    // // sh "sudo touch /etc/apt/sources.list.d/kubernetes.list "
                    // // sh echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
                    // // sh "apt update && apt install -y kubectl"
                    // sh 'curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl'
                    // sh "chmod +x ./kubectl && mv ./kubectl /usr/local/bin"
                    // sh "curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3"
                    // sh "sudo chmod 700 get_helm.sh"
                    // sh "./get_helm.sh"
                    // // withCredentials([file(credentialsId: 'k3d-shalini02', variable: 'KUBECRED')]) {
                    // //     // change context with related namespace
                    // //     sh 'mkdir ~/.kube'
                    // //     sh 'cat $KUBECRED > ~/.kube/config'
                    // //     // sh 'KUBE_API_PORT="--port=8080"'
                    // //     sh "kubectl config view"
                    // //     // sh "kubectl config use-context rancher-desktop"
                    // //     sh "kubectl get nodes"
                    // // }
                    // // sh "helm upgrade --install --set deployment.image=${dockerImage} --set secret.securestring=${IMAGEPULL_SECRET} ${HELM_RELEASE} ./helm-deployment"
                    // sh "helm upgrade --install --force --set deployment.image=${dockerImage} --set secret.securestring=${IMAGEPULL_SECRET} ${HELM_RELEASE} ./helm-deployment"                 
                }
            }
        }
    }
}
