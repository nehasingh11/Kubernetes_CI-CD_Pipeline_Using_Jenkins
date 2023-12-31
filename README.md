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

   sudo apt-get update
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

   sudo groupadd docker
   sudo usermod -aG docker $USER
   newgrp docker
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
sudo adduser dmistry
cd ~
mkdir .kube
cd .kube/
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
10. create a new file on argoCD VM /root/.kube/app-cluster.yml and paste the content from above step. Change the ip of application VM
11. export KUBECONFIG=~/.kube/app-cluster.yml
12. Install ArgoCD command line tool

curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64

13. argocd login 172.172.147.8:30952 (use the ports shown in step 6)
    username - admin
    password - step 5 output
14. Add the other VM
    argocd cluster add default --name vm2


================== Install Prometheus and Grafana===================
1. Install Prometheus plugin in jenkins (Prometheus metrics plugin)
2. Under manage jenkins -> system configuration -> prometheus -> edit the configuration if required
3. This plugin will open an end point for Prometheus to extract the data i.e jenkinsIP:8080/prometheus (http://34.125.101.157:8080/prometheus/)
4. Install Prometheus
   docker run -d --name prometheus -p 9090:9090 prom/prometheus
5. Install Grafana
   docker run -d --name grafana -p 3000:3000 grafana/grafana
6. Integrate Prometheus with jenkins:
   enter into the prometheus container
   docker exec -it 1654a707a6b2 /bin/sh
   cd /etc/prometheus/
   vi prometheus.yml -> edit the file

    - job_name: "jenkins"
    metrics_path: /prometheus
    static_configs:
     - targets: ["34.125.101.157:8080"]   (jenkins IP)
8. docker restart 1654a707a6b2
9. Access the prometheus on 9090
10. Access grafana on 3000
    - Add Data source
    - Select Prometheus
    - Give the Prometheus Server URL
    - Save and Test
    - create new dashboards 
