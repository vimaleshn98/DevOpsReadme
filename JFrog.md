### Creating Account
Link : https://jfrog.com/artifactory/

Choose Cloud Version

ServerName : harvey

You will get a mail:
```
Platform URL: https://harvey.jfrog.io
Username: samalatib96@gmail.com
Password: As you provided in the registration form
```
Once Activated:
```
WE'RE GETTING YOUR JFROG PLATFORM READY!
Your DevOps Platform services are being finalized. This will only take a few moments.
Server Name:
harvey
Location:
AWS in Mumbai
```

### Creating Repository

In Repositories Create a new Repo in Local

Choose Docker Type and give Name and Create.

Convention : art-doc-dev-local


* FOR DOCKER USE ONLY
```
According to the repository permission, you will need to login to your repository with docker login command


docker login harvey.jfrog.io
Pull an image.


docker pull hello-world
Tag an image.


docker tag hello-world harvey.jfrog.io/art-doc-dev-local/hello-world
Then push it to your repository.


docker push harvey.jfrog.io/art-doc-dev-local/hello-world

```


### Store Artifacts Within Jenkins Workspace
```
stage('collect-artifacts'){
            steps{
                archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
            }
        }
```
```
post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                       }
         
            }
```

## Pipeline Integration

### Jenkins Configuration
Config Sys > jFrog > Arti Servers
```
Server ID : artifactoryserver 
URL : https://harvey.jfrog.io/artifactory
(platform URL/artifactory)

Default Deployer Credentials
UserName : jFrog Username
Password : jFrog Password
```

Then Test the Connection
```
Output:	
Found Artifactory 7.16.3
```

### Upload Script
```
        stage ('deploy to artifactory'){
            steps{
                script{
                    last_started=env.STAGE_NAME
                }
                rtUpload(
                    serverId: 'artifactoryserver',
                    spec: '''{
                        "files":[
                            {
                              "pattern": "target/*.jar",
                                "target": "art-doc-dev-local1"
                             }
                        ]
                    }''',
                )
            }
        } 
```

### Download Script
```
        stage ('download to artifacts folder'){
            steps{
                script{
                    last_started=env.STAGE_NAME
                }
                rtDownload(
                    serverId: 'artifactoryserver',
                    spec: '''{
                        "files":[
                            {
                              "pattern": "art-doc-dev-local1/",
                                 "target": "artifacts/"
                             }
                        ]
                    }''',
                )
            }
        }
```


