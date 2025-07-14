# FirstJAVAWebAppHelloWorld
This project involves building a basic web application using Java and Apache Maven. The primary objective is to set up a minimal Maven-based Java project structure and configure it to deploy a web application that simply displays the message 'Hello World' in a web browser.

MAVEN:
Maven is a powerful open-source build automation tool widely used in DevOps to streamline the software development lifecycle, especially for Java projects. It offers features like dependency management, build lifecycle management, and plugin support, enabling efficient, consistent, and automated build and deployment processes. Key roles of Maven are:
- Declarative, Consistent Builds:  
  Maven’s pom.xml defines every project’s build steps so they run the same way everywhere.
- Automated Dependency Management:  
  It downloads and locks library versions for you, avoiding manual jar handling.
- Opinionated Project Layout:  
  A fixed directory structure ensures source code, tests and resources live in predictable places.
- Lifecycle Automation via Plugins:  
  Standard phases—compile, test, package, etc.—are wired to plugins so you don’t write custom scripts.
- Versioned Artifact Production:  
  Each build outputs a uniquely versioned JAR or WAR ready for deployment.

Importance in DevOps:
- CI/CD Enabler:  
  Integrates with Jenkins, GitLab CI, etc., to run builds, tests and packaging automatically.
- Reproducibility:  
  Locked dependencies and standardized builds eliminate “it works on my machine” problems.
- Faster Feedback & Delivery:  
  Plugin‑driven workflows accelerate the build→test→package cycle.
- Seamless Integrations:  
  Hooks into Nexus/Artifactory, scanners and deployment tools out of the box.
- Team Consistency:  
  A single pom.xml means everyone shares the same build contract.

Common Commands:
mvn clean                    #Delete target directory  
mvn compile                  #Compile source code  
mvn test                     #Run unit tests  
mvn install                  #Install to local repository  
mvn dependency:tree          #View dependency tree  

# Installing and Configuring Apache Maven:

## Prerequisites:
- Create a VM named ‘developer’
- Maven requires Java (JDK 11+)

Install Java and verify version:
```bash
sudo dnf list | grep jdk
sudo dnf -y install java-21-openjdk
java -version

cd /usr/lib/jvm
ls
cd java-21-openjdk…
pwd   # /usr/lib/jvm/java-21-openjdk
```
Set JAVA_HOME systemwide:
```bash
sudo vi /etc/profile
# add:
export JAVA_HOME=/usr/lib/jvm/java-21-openjdk
source /etc/profile
echo $JAVA_HOME
```

## Download and Install Maven:
```bash
sudo dnf list maven
# Or download latest from Apache site:
cd /opt
sudo mkdir maven
cd maven
sudo wget https://dlcdn.apache.org/maven/maven-3/3.9.10/binaries/apache-maven-3.9.10-bin.tar.gz
sudo tar -zxvf apache-maven-3.9.10-bin.tar.gz

cd /opt/maven/apache-maven-3.9.10/bin
ls
cat mvn   # mvn is the shell script launcher for Maven

# Add mvn to PATH:
sudo vim /etc/profile
# add:
export PATH=$PATH:/opt/maven/apache-maven-3.9.10/bin
export M2_HOME=/opt/maven/apache-maven-3.9.10
source /etc/profile

echo $M2_HOME
which mvn
mvn --version
```

# Create a Java application using Maven:
```bash
mkdir -p ~/javaapp
cd ~/javaapp

# Step 1: Create a project structure
mvn archetype:generate   -DgroupId=com.devopsclass   -DartifactId=HelloWorld   -DarchetypeArtifactId=maven-archetype-webapp   -DinteractiveMode=false

tree HelloWorld
```

**Explanation: Maven Project Creation**
- `mvn archetype:generate` – Invokes Maven to create a project using a template.  
- `-DgroupId=com.devopsclass` – Defines base package (reverse domain format).  
- `-DartifactId=HelloWorld` – Sets project name and root directory.  
- `-DarchetypeArtifactId=maven-archetype-webapp` – Uses a basic webapp WAR template.  
- `-DinteractiveMode=false` – Skips prompts and uses given values directly.

Common Maven Archetypes for Production Environments:

| Category             | Archetype ID                    | Framework / Purpose                 | Production Use Case                       |
|----------------------|---------------------------------|-------------------------------------|-------------------------------------------|
| Standard             | maven-archetype-quickstart      | Simple Java project (JAR)           | Microservices, libraries                  |
|                      | maven-archetype-webapp          | Basic web application (WAR)         | Legacy web apps                           |
| Framework-Specific   | spring-boot-starter             | Spring Boot                         | Modern microservices, web applications    |
|                      | quarkus-usecase                 | Quarkus                             | Cloud-native apps, Kubernetes             |
|                      | micronaut                       | Micronaut                           | Lightweight, serverless applications      |
| Enterprise Java      | jakartaee-10-archetype          | Jakarta EE 10                       | Modern enterprise Java applications       |

```bash
cd HelloWorld
ls
# pom.xml  src/main/webapp/index.jsp
```

## SDLC:
Requirements gathering & analysis → Design → Implementation → Testing → Deployment → Maintenance & Operations.

### pom.xml:
`pom.xml` (Project Object Model) is the core configuration file for Apache Maven projects. It defines a project's structure, dependencies, build settings, and metadata using XML format. Every Maven project must include this file at its root.

# Compiling code
```bash
cd HelloWorld
mvn compile
```
> **Note:**  
> - Run inside the project directory where `pom.xml` resides.  
> - Maven will download required plugins automatically to `~/.m2/repository`.

# Packaging
`mvn package` compiles your Java code, runs unit tests, and bundles the compiled code and resources into a WAR. The final artifact is saved in `target/`.

```bash
mvn package  # First run downloads plugins
ls target    # see HelloWorld.war
```

# Deploy to Tomcat
```bash
# On your Tomcat server:
# Ensure Tomcat is powered on.

# On your developer machine:
cd ~/javaapp/HelloWorld/target
scp HelloWorld.war cnode@10.10.5.9:~/apache-tomcat-9.0.106/webapps
```
Default WAR location on Tomcat is `webapps`.

Now access the app in your browser:
```
http://10.10.5.9:5080/HelloWorld/
```
