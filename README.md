### Automated Deployment: Docker, Jenkins, SonarQube & Trivy for a Static website: CycleKart

## Introduction

In modern web development, automating deployment and ensuring your code quality is crucial for maintaining high standards and reducing errors. This guide walks through automating the deployment of a static website using **Docker**, **Jenkins**, **SonarQube**, and **Trivy**.

By the end, you'll have a CI/CD pipeline that:

- Builds and tests your website  
- Runs static analysis via SonarQube  
- Scans for vulnerabilities with Trivy  
- Deploys the site to a Docker container

---

## Why Use Docker, Jenkins, SonarQube & Trivy?

- **Docker** ensures consistent environments  
- **Jenkins** automates the build, test, and deploy cycle  
- **SonarQube** analyzes code quality and enforces a Quality Gate  
- **Trivy** performs security scans on both the file system and Docker images  

---

## Step 1: Prepare Your Static Website (CycleKart)

Your website should be a simple HTML/CSS/JS project.

**Tools Required:**

- Static website files (HTML/CSS/JS)  
- GitHub repository (for Jenkins integration)

Make sure the website runs locally before proceeding.

---

## Step 2: Dockerize the Static Website

```bash
docker build -t bike-app-store .
docker run -d -p 8080:80 bike-app-store
Visit http://localhost:8080 to test locally.
```
--- 

##  Step 3: Set Up Jenkins

Jenkins automates the CI/CD pipeline — from pulling code from GitHub to deploying Docker containers.

### Install Jenkins on Ubuntu

```bash
sudo apt update
sudo apt upgrade -y

sudo apt update
sudo apt install fontconfig openjdk-17-jre -y
java -version

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins

sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```

### Jenkins plugins: 
From Jenkins Dashboard → Manage Jenkins → Manage Plugins, install:
- Pipeline
- Docker Pipeline
- NodeJS
- Maven
- SonarQube Scanner
- Extended Email
- Pipeline Stage View
- Eclipse Temurin

##  Step 4: Add SonarQube & Trivy to the Jenkins Pipeline

To ensure your code is high-quality and secure, we'll integrate **SonarQube** for static code analysis and **Trivy** for vulnerability scanning.

###  Prerequisites

- SonarQube installed and accessible from Jenkins  
- Trivy installed on the Jenkins server

### Install Trivy (on Ubuntu)

```bash
wget https://github.com/aquasecurity/trivy/releases/latest/download/trivy_0.50.1_Linux-64bit.deb
sudo dpkg -i trivy_0.50.1_Linux-64bit.deb

trivy --version 
```
## Step 5: Enable Continuous Deployment (CD)

Once your Jenkins pipeline is configured and connected to GitHub, every code push triggers the following stages automatically:

### Automated Workflow

1. **Code Push to GitHub**  
2. **Jenkins Pipeline Triggers Automatically**
3. **Pipeline Steps Execute Sequentially:**
    - Clean workspace
    - Checkout latest code from GitHub
    - Analyze code with **SonarQube**
    - Enforce **Quality Gate**
    - Run **Trivy** file system vulnerability scan
    - Build Docker image
    - Push Docker image to Docker Hub
    - Run **Trivy** image vulnerability scan
    - Deploy container on port `8080`

---

## ✅ Final Outcome

With this setup, your static website:

- Is **containerized** using Docker
-  Is **analyzed** for code quality via SonarQube
-  Is **scanned** for vulnerabilities using Trivy
-  Is **deployed automatically** via Jenkins every time you push to GitHub
