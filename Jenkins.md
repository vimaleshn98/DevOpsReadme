# Jenkins Insatllation as Docker Image
### General Image Pull
```
docker run -p 8080:8080 -p 50000:50000 jenkins
```
This will store the workspace in /var/jenkins_home. All Jenkins data lives in there - including plugins and configuration. You will probably want to make that a persistent volume (recommended):
```
docker run -p 8080:8080 -p 50000:50000 -v /your/home:/var/jenkins_home jenkins
```
This will store the jenkins data in /your/home on the host. Ensure that /your/home is accessible by the jenkins user in container (jenkins user - uid 1000) or use -u some_other_user parameter with docker run.

You can also use a volume container:
```
docker run --name myjenkins -p 8080:8080 -p 50000:50000 -v /var/jenkins_home jenkins
```
Then myjenkins container has the volume (please do read about docker volume handling to find out more).

Copy the passkey generated in cmd and use it during running Jenkins for the first time.

Set up the jenkins with name mail and password.

### Docker in Docker
stop all docker images running on port 8080 or change the port number

1.Go to your cmd run this command
```

docker run -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock --name jenkins jenkins/jenkins:lts

```
we will get an initial admin password do the jenkins installation steps 

After completion of jenkins installation

2.To enter into your docker image run this command
```
docker exec -it -u root jenkins bash

```

3.install docker in docker
```
apt-get update && \
apt-get -y install apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     software-properties-common && \
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey; apt-key add /tmp/dkey && \
add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
   $(lsb_release -cs) \
   stable" && \
apt-get update && \
apt-get -y install docker-ce
```


now you will be inside your docker and run '''docker ps''' you will get something like this
```
root@cf1fd5908a1c:/# docker ps
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                               NAMES
cf1fd5908a1c        jenkins/jenkins:lts   "/sbin/tini -- /usr/…"   2 hours ago         Up 12 minutes       0.0.0.0:8080->8080/tcp, 50000/tcp   jenkins
root@cf1fd5908a1c:/#   
```
this means you have succesfully created the image 

Now to give jenkins actions access to all users for that run this command
```
chmod 777 /var/run/docker.sock
```
run your jenkins pipeline
```
pipeline{
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }
  }
  stages {
    stage('Build'){
      steps{
        sh 'mvn -B -DskipTests clean package'
      }
     }
    stage('Test'){
      steps{
        sh 'mvn test'
      }
      
  }
  }
}
```

# VS Code Integration for Pipeline Script

Install Jenkins Pipeline Linter Connector Plugin in VsCode

Configure As Follows :
Crumb URL
```
http://localhost:8080/crumbIssuer/api/xml?xpath=concat
```
Pass
```
Protagonist@2504
```
URL
```
http://localhost:8080/pipeline-model-converter/validate
```
User
```
Harvey25
```
Shortcut to validate a Jenkinsfile
```
Shift+Alt+V
```

# Basic Plugins Installation
Manage Jenkins > Manage Plugins

Install Basic plugins for :
*   Dark Theme
*   Email Ext
*   Git
*   Sonarqube
*   Artifactory
*   Amazon Web Services
*   SSH Plugins
*   Terraform
*   Docker
*   Kubernetes

#   Set Up Java and Maven Configs

# Pipeline Scripting

# Sample Config : Mail Config