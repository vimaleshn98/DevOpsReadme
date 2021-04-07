# About Maven
POM : Project Object Model
```
mvn clean 
```
Cleans the files and directories created by previous maven builds
```
mvn validate
```
Checks the configuration and dependencies in pom file,etc
```
mvn compile
```
Generates the .class file
```
mvn test
```
Unit tests are carried on with Ex:jUnit

Also creates test classes

Reports are stored in target > surefire reports folder

To skip tests : -DskipTests  
```
mvn package
```
Extracts the .jar or .war file as per the given specs in pom.xml

In other words , packages files/application into distributable executable jar/war.

Stores in target folder
```
mvn install
```
Installs SNAPSHOTS into .m2 folder repo of local machine

Keeps a copy in local syatem
```
mvn deploy
```
Deploy to Repo using UName , Pass ,etc (Credentials)


Sometimes the build might fail during compiling the project at mvn compile stage
```
<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
 <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
```
Paste these three lines in pom.xml properties part

Try this and compile again.