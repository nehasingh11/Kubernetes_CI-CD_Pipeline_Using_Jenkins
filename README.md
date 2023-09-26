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
