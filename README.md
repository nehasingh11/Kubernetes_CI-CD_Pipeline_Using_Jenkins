# Sample application for e2e DevOps Pipeline

## This is a sample application to demonstrate an end to end DevOps Pipeline

1. Script to install nginx and jenkins

#!/bin/bash
sudo apt update
sudo apt upgrade -y
sudo apt install nginx -y
systemctl status nginx
sudo apt install -y openjdk-17-jdk
sudo apt install -y openjdk-17-jre
sudo apt-get install git-all -y
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo 'deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/' | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update && sudo apt-get install jenkins -y
sudo service jenkins restart
sudo apt-get install python3 -y

2. For SonarQube Quality Gate create a webhook in SonarQube:

   Under Administration -> Configurations -> Webhooks
   name - jenkins.cloud
   url - http://20.169.188.118:8080/sonarqube-webhook/ (IP of jenkins server)
   secret - no

3. How to install Docker:
   Install using the repository¶
   Update the apt package index and install packages to allow apt to use a repository over HTTPS:

   sudo apt-get update

   sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

   Add Docker’s official GPG key:

   sudo mkdir -m 0755 -p /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

   Use the following command to set up the repository:

   echo \
   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

   Install Docker Engine
   sudo apt-get update
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

   Manage Docker as a non-root user¶
   Create the docker group.
   sudo groupadd docker

   Add your user to the docker group.
   sudo usermod -aG docker $USER

   Run the following command to activate the changes to groups:
   newgrp docker

   Verify that you can run docker commands without sudo.
   docker run hello-world

Notes:

1. Plugins for sonarqube:
   Quality Gates Plugin
   Sonar Quality Gates Plugin
   SonarQube Scanner for Jenkins
2. Plugins for Maven:
   Maven Integration plugin
   Pipeline Maven Integration Plugin
3. Plugins for JDK:
   Eclipse Temurin installer Plugin

4. Plugins for Docker:
   CloudBees Docker Build and Publish plugin
   Docker API Plugin
   Docker Commons Plugin
   Docker Pipeline
   Docker plugin
   docker-build-step

5. Add token for Docker and github using username and password (generate token)

ArgoCD

Create two VMs in the same vnet
Follow below steps on two VMs.

1. sudo apt update
   sudo apt upgrade

2.sudo bash
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="server" sh -s - --disable traefik
exit
mkdir .kube
sudo cp /etc/rancher/k3s/k3s.yaml ./config
sudo chown dmistry:dmistry config
chmod 400 config
export KUBECONFIG=~/.kube/config

Install argoCD on one VM

3. kubectl create namespace argocd
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

4. kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
5. kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
6. kubectl get svc -n argocd
7. kubectl get pods -n argocd
8. open the ports on the argoCD server to access using web browser

9. Copy the content of /root/.kube/config from application vm (second node)
10. create a new file on argoCD VM /root/.kube/app-cluster.yml and paste the content from above step.
11. export KUBECONFIG=~/.kube/app-cluster.yml
12. argocd login 172.172.147.8:30952 (use the ports shown in step 6)
    username - admin
    password - step 5 output
13. Add the other VM
    argocd cluster add default --name vm2
