# About Maven
POM : Project Object Model
```
mvn clean 
```
Cleans the files and directories created by previous maven builds
```
mvn validate
```
Checks the configuration and dependencies
```
mvn compile
```
Generates the .class file
```
mvn test
```
Unit tests are carried on with Ex:jUnit

Reports are stored in target > surefire reports folder
```
mvn package
```
Extracts the .jar or .war file as per the given specs in pom.xml

Stores in target folder
```
mvn install
```
Installs SNAPSHOTS into .m2 folder repo of local machine