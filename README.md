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
