## Sonarqube Cloud
* Open https://sonarcloud.io/
* Open with github Repo
* Add Button on top right
* Analyze new Project (If organisation is already created)
* Select Project and set up
* Click on Manually tab and select Maven option for java apps
* Copy the generated token for project (value of environment variable to be used for pipeline integration)
```
Name of the environment variable: SONAR_TOKEN 
Value of the environment variable: 4ebc343ad863034124b4ae3fe3806a9a5603c77e
```
* Copy following properties and update the pom.xml file with it
```
<properties>
  <sonar.projectKey>Harvey2504_SpringCopy_Docker4</sonar.projectKey>
  <sonar.organization>myfirstappinqube</sonar.organization>
  <sonar.host.url>https://sonarcloud.io</sonar.host.url>
</properties>
```
* Run this command in project folder
```
mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar
```
* Is it done? You should see the page refresh itself in a few moments with your analysis results if everything runs successfully.
* Set a new code definition for sonar gate in the project once a run is successful to generate a quality gate based on specifications. 


## Pipeline Integration
* Editing pom.xml (as done earlier)
* Creating Credentials
```
Manage Jenkins > Manage Credentials 
Add Credentials
kind : Secret Text
Secret : token generated from environment variable
ID : sonarqube1
Description : sonarqube1
```
* Config Sonar Server
* Sonar Analysis Pipeline Script
* Quality Gate Pipeline Script



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