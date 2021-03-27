## VS Code Integration
Connecting ubuntu instance with vscode

Keep the .pem file inside .ssh folder in computer

Download Remote SSH Plugin on VSCode

Add to SSH Targets

Then Type
ssh -i ~/.ssh/awskeypair.pem  ubuntu@ec2-52-14-186-207.us-east-2.compute.amazonaws.com

Hit Enter twice 

Allow Connections

Once the instance is added to targets list right click and open in new window 

Press Okay

Once instance is online , goto File>Open Folder and click on the Home/Ubuntu Directory

## Putty Connection
Link:
https://www.youtube.com/watch?v=bi7ow5NGC-U

* Converting pem file

open putty gen 

load the keypair (all files)

save as private key with a name

* Launch Putty
on session fill in host id

Connection>Data : type the user name of instance

Connection>SSH>Auth : browse and give the converted key

Save the Session with a name


## RDP Client Set Up


## RDS Creation


## SSH Agent Integration

## SSH Agent Test Validation
Try this in pipeline script of jenkins (if error arises during ssh connection)
```
pipeline {
    agent any

    stages {
        stage('test') {
            steps {
                sshagent(['73480030-7719-475d-8bb3-d7a4fc22c524']){
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@3.18.221.13 pwd'
                }
                
            }
        }
    }
}
```
```
OP : Warning: Permanently added '3.18.221.13' (ECDSA) to the list of known hosts
```



## Pipeline Script for Deployment
### For Local jenkins
```
 stage('aws deployment'){
            steps{
                script{
                    last_started=env.STAGE_NAME
                }
                sshagent(['f17813c1-8ef1-4fee-960e-74e7c6f7efcb']){
                    bat 'scp -r C:/Windows/System32/config/systemprofile/AppData/Local/Jenkins/.jenkins/workspace/ContinuousIntegrationPipeline/artifacts/*.jar ubuntu@3.18.221.13:/home/ubuntu/artifacts'
        }
            }
        } 
```

### For Jenkins on Docker
```
   stage('aws deployment'){
            steps{
                script{
                    last_started=env.STAGE_NAME
                }
                sshagent(['73480030-7719-475d-8bb3-d7a4fc22c524']){
                    sh 'scp -r /var/jenkins_home/workspace/ContIntegEmployee/artifacts/*.jar ubuntu@3.18.221.13:/home/ubuntu/artifacts'
        }
            }
        } 
```


##### Explore ec2 ,s3 ,vpc ,subnet and sec_groups.