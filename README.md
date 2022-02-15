# Title

Implementing an end to end CI/CD workflow using Jenkins for deploying containerized applications to Kubernetes.

## About the project

In this project, a simple tomcat application has been containerized using docker and deployed to kubernetes using Jenkins pipelines. 
Testing was done on local k3s Rancher Desktop, k3d for Docker Desktop and Azure AKS cluster with successful results.
Helm deployment has been used for Jenkins and app deployment.
The Jenkins instance and the app could be exposed publicly by binding valid SSL certificates with Ingress controller kubernetes objects while deploying to actual production scenarios. Also, update the Helm values for the app in that case.
This is however not part of this sample setup. All testing was done locally.

## Prerequisites

Below tools are needed to work with this project

```
Kubernetes cluster
Docker / Rancher Desktop
Traefik ingress Controller (setup could be tweaked for nginx with a few changes)
Helm
```

## Setting up

Setup Traefik Ingress Controller on the kubernetes cluster using below Helm chart.
Rancher desktop provide a local kubernetes cluster with a Traefik controller out of the box.

```
helm repo add traefik https://helm.traefik.io/traefik
helm repo update
helm install traefik traefik/traefik
```

For deployment of Jenkins instance to your kubernetes cluster, run the below command.
The values file could be adjusted as per your requirement (if needed).

```
helm repo add jenkins https://charts.jenkins.io
helm repo update
helm upgrade --install myjenkins jenkins/jenkins --values jenkins-helm-values.yaml
```

Setup RBAC for default service account on the kuberentes cluster

```
kubectl create clusterrolebinding serviceaccounts-cluster-admin-default \
  --clusterrole=cluster-admin \
  --serviceaccount=default:default
```
Per the jenkinsfile environment variables used, below 3 environment secrets are needed within the Jenkins project.

dockersecret : Secret text for use in kubernetes deployment to pull images from docker hub private repository.
Use below command to generate the base64 encoded secret.

```
kubectl create secret docker-registry registrypullsecret --docker-server=https://index.docker.io/v1/ --docker-username=REPLACE_ME --docker-password=REPLACE_ME --docker-email=REPLACE_ME --dry-run -o yaml
```
dockerhub-push : Username with password for pushing images to docker hub
kubeconfig : Secret file. Provide the kubeconfig file for your target deployment cluster where the deployment should be done.


## Running the Jenkins pipeline

Create a new multibranch pipeline using your GIT source repository.
As per the current jenkinsfile, all deployments to main branch or branches which contain features/develop in their name will trigger a build automatically.

## Continuous deployment

Setting a value of 'false' for deployToLocal environment variable in Jenkinsfile deploys the application on the same kubernetes cluster where the Jenkins instance is setup.
To select a different target environment for your deployment, update the kubeconfig variable in Jenkinsfile with the ID of a kubeconfig secret file