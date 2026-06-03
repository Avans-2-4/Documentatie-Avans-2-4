For excercise 2 we had to install [[SonarQube]]. We had already done that. (SonarQube Cloud). SonarQube cloud scanned all our github repo's and have a resulted our statistics. You can view that [here](https://sonarcloud.io/organizations/avans-2-4/projects).

But the Softwaredesign-en-kwaliteit-Avans2-4LU2 repo was not scanned. This is because sonarqube didn't recognise the project as a java project. This stems from a Java project needing to be either a Gradle or Maven project, and our project was neither.

So to fix this I added a manual analysis of the project via CI/CD.

## Manual configuration


### Turning java app into maven

Just follow the [IntelliJ Guide](https://www.jetbrains.com/help/idea/convert-a-regular-project-into-a-maven-project.html), rough steps:
- add a pom.xml at root
- click the "load maven project" pop-up
- add the default XML content: 
```xml
  <project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>cool-name</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>Cool Name</name>
</project>
```
- click the "sync maven project" button
- profit

### Adding SonarQube CICD

In the maven dashboard, open the project. It'll tell you something along the lines of "*The last attempt wasn't successful because this project didn't meet the [compatibility criteria (opens in new tab)](https://docs.sonarsource.com/sonarqube-cloud/analyzing-source-code/automatic-analysis/#considerations).*"
- Click the "With GitHub Actions" button / link.
- Add the github secret it tells you
- select Maven as option
- add the section to your pom.xml
- add the example build workflow.yml
