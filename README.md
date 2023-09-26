# Sample application for e2e DevOps Pipeline

## This is a sample application to demonstrate an end to end DevOps Pipeline

For SonarQube Quality Gate create a webhook in SonarQube:

1. Under Administration -> Configurations -> Webhooks
   name - jenkins.cloud
   url - http://20.169.188.118:8080/sonarqube-webhook/ (IP of jenkins server)
   secret - no
