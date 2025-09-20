# Sonar_assign
Assignment 15/09/2025
 Deploy a SonarQube server, integrate it with a sample application and perform static code analysis.
 
Assignment 16/09/2025 - In continuation to previous assignment

1.⁠ ⁠Trigger a jenkins pipeline on new branch named (name.developer) using webhooks.
2.⁠ ⁠⁠Pull latest code from Github
3.⁠ ⁠⁠Setup  Sonar-Qube on K8s cluster and add a stage in jenkins to scan the code after checkout out
4.⁠ ⁠⁠If Sonar-Qube test successfully completes, then build and generate artifacts .
-
-
-
-
-

Sonar_assign/
 ├── pom.xml
 └── src
     ├── main
     │   └── java
     │       └── com
     │           └── example
     │               └── Hello.java
     └── test
         └── java
             └── com
                 └── example
                     └── HelloTest.java





Step 1: Deploy SonarQube Server
Option A: Run with Docker (Recommended for quick setup)
# Create a Docker network
docker network create sonarnet


# Run PostgreSQL (SonarQube requires a DB)
docker run -d --name sonardb --network sonarnet \
  -e POSTGRES_USER=sonar \
  -e POSTGRES_PASSWORD=sonar \
  -e POSTGRES_DB=sonar \
  postgres:13
  

# Run SonarQube
docker run -d --name sonarqube --network sonarnet \
  -p 9000:9000 \
  -e SONAR_JDBC_URL=jdbc:postgresql://sonardb:5432/sonar \
  -e SONAR_JDBC_USERNAME=sonar \
  -e SONAR_JDBC_PASSWORD=sonar \
  sonarqube:community


👉 SonarQube will be available at:
http://localhost:9000

Default login: admin / admin

Step 2: Setup SonarQube Project

Login to SonarQube UI.

Go to Projects → Create Project.

Select Manually → Enter project key & name (e.g., sample-app).

Choose Generate Token → copy it (you’ll need this for scanner).



Step 3: Install SonarScanner

On your development/CI server:

# Linux
wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-5.0.1.3006-linux.zip
unzip sonar-scanner-*.zip -d /opt
export PATH=$PATH:/opt/sonar-scanner-*/bin


Check installation:

sonar-scanner -v



Step 4: Sample Application (Java Example)

Let’s use a simple Maven-based Java app.

git clone github url


Create a file sonar-project.properties in the root:

sonar.projectKey=sample-app
sonar.projectName=Sample Application
sonar.projectVersion=1.0
sonar.sources=src
sonar.language=java
sonar.sourceEncoding=UTF-8

# SonarQube server details
sonar.host.url=http://localhost:9000
sonar.login=<YOUR_GENERATED_TOKEN>

Step 5: Run Static Code Analysis
sonar-scanner

This will:

Scan the code

Send results to SonarQube server

You can see issues under Dashboard → sample-app


mvn clean verify sonar:sonar \
  -Dsonar.projectKey=Sample_app \
  -Dsonar.projectName='Sample_app' \
  -Dsonar.host.url=http://184.73.14.110:9000 \
  -Dsonar.token=sqp_b0abb072ee9e184322faa9cdfb0fcb5bbd612494

