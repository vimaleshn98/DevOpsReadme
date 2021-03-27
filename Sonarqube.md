sonarqube cloud
project addition and token gen
new code definition
quality gate generation

pipeline Integration :
editing pom.xml
creating user credentials
sonar analysis
pipeline script for quality gate


```
 <sonar.projectKey>Harvey2504_Sample_Calculator_App</sonar.projectKey>
  <sonar.organization>myfirstappinqube</sonar.organization>
  <sonar.host.url>https://sonarcloud.io</sonar.host.url>
```

* Quality Gate for Scripted Pipeline (Not Recommended)
```
 stage("Quality Gate"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }
```