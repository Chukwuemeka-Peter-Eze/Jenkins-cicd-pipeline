# SETUP GUIDE

> Step-by-step instructions for provisioning the Jenkins Continuous Integration environment on an AWS EC2 Ubuntu Server.

---

# Project Overview

This guide explains how to build the complete development environment used in this project.

By the end of this guide you will have:

- Ubuntu Server running on AWS EC2
- Java installed
- Maven installed
- Git installed
- Docker installed
- Jenkins installed
- Sonatype Nexus Repository running in Docker
- Jenkins connected to GitHub
- Jenkins connected to Docker
- Jenkins connected to Nexus Repository
- A fully functioning Continuous Integration pipeline

---

# Environment Specifications

| Component | Version |
|------------|----------|
| Cloud Provider | AWS |
| Operating System | Ubuntu Server |
| Jenkins | Latest LTS |
| Java | JDK 17 |
| Maven | 3.x |
| Docker | Latest Stable |
| Nexus Repository | Sonatype Nexus 3 |
| Git | Latest Stable |

---

# Architecture

```text
AWS EC2
│
├── Ubuntu Server
│
├── Git
│
├── Java
│
├── Maven
│
├── Docker
│
├── Jenkins
│
└── Nexus Repository
```

---

# Step 1: Launch an AWS EC2 Instance

Create a new EC2 instance with the following recommended configuration:

| Setting | Value |
|----------|-------|
| AMI | Ubuntu Server LTS |
| Instance Type | t2.medium (or higher if resources allow) |
| Storage | 20 GB or more |
| Security Group | Allow required ports (SSH, Jenkins, Nexus) |

After the instance is running, connect using SSH.

```bash
ssh -i your-key.pem ubuntu@<EC2-PUBLIC-IP>
```

---

# Step 2: Update the Server

Update package indexes and install the latest available updates.

```bash
sudo apt update
sudo apt upgrade -y
```

Verify the operating system:

```bash
cat /etc/os-release
```

---

# Step 3: Install Git

Install Git:

```bash
sudo apt install git -y
```

Verify installation:

```bash
git --version
```

---

# Step 4: Install Java

Install Java Development Kit (JDK):

```bash
sudo apt install openjdk-17-jdk -y
```

Verify:

```bash
java -version
```

---

# Step 5: Install Maven

Install Maven:

```bash
sudo apt install maven -y
```

Verify:

```bash
mvn -version
```

---

# Step 6: Install Docker

Install Docker:

```bash
sudo apt install docker.io -y
```

Enable Docker:

```bash
sudo systemctl enable docker
```

Start Docker:

```bash
sudo systemctl start docker
```

Verify:

```bash
sudo systemctl status docker
```

---

# Step 7: Configure Docker Permissions

To run Docker commands without `sudo`, add your user to the Docker group:

```bash
sudo usermod -aG docker $USER
```

Apply the new group membership by logging out and back in, or by starting a new session.

Verify:

```bash
docker ps
```

If you receive a "permission denied" error, ensure your group membership has taken effect before proceeding.

---

# Step 8: Install Jenkins

Add the Jenkins repository and install the Jenkins package according to the official installation instructions.

After installation:

```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

Verify:

```bash
sudo systemctl status jenkins
```

Open the Jenkins web interface using your browser:

```text
http://<EC2-PUBLIC-IP>:8080
```

Complete the initial setup wizard and install the recommended plugins.

---

# Step 9: Install Required Jenkins Plugins

Install the plugins used in this project.

Recommended plugins include:

- Git
- GitHub
- Pipeline
- Pipeline Utility Steps
- Docker Pipeline
- Credentials
- Maven Integration

After installation, restart Jenkins if prompted.

---

# Step 10: Configure Global Tools

Navigate to:

```
Manage Jenkins
→ Tools
```

Configure:

- Git
- JDK
- Maven

Use meaningful tool names that match those referenced in your Jenkins jobs or pipelines.

---

# Step 11: Configure GitHub Credentials

Create credentials for accessing your GitHub repository.

Navigate to:

```
Manage Jenkins
→ Credentials
```

Add the appropriate credential type (for example, username/password or personal access token, depending on your setup).

Assign a clear **Credential ID** so it can be referenced by Jenkins jobs.

---

# Step 12: Clone the Repository

Clone the project:

```bash
git clone https://github.com/<your-username>/Jenkins-cicd-pipeline.git
```

Move into the project directory:

```bash
cd Jenkins-cicd-pipeline
```

---

# Step 13: Run Sonatype Nexus Repository

Start Nexus in Docker using a persistent volume.

Example:

```bash
docker volume create nexus-data

docker run -d \
  --name nexus \
  -p 8081:8081 \
  -p 8083:8083 \
  -v nexus-data:/nexus-data \
  sonatype/nexus3
```

Confirm the container is running:

```bash
docker ps
```

Wait for Nexus to complete its initial startup before logging in.

---

# Step 14: Configure Nexus

After Nexus is available:

- Sign in using the administrator account.
- Change the default password.
- Create the required Docker hosted repository.
- Configure the repository settings to match your pipeline.

Document the repository name and endpoint, as they will be used by Jenkins during image publication.

---

# Step 15: Configure Docker for the Registry

If your registry uses HTTP, configure Docker appropriately before attempting to authenticate and push images.

Restart Docker after making configuration changes.

Verify connectivity to the registry before continuing.

---

# Step 16: Create a Jenkins Job

Create either:

- Freestyle Project
- Pipeline Project

Configure:

- Source Code Management (GitHub)
- Build Triggers (optional)
- Build Steps
- Credentials
- Pipeline definition (for Pipeline jobs)

---

# Step 17: Execute the Pipeline

Run the job.

A successful execution should perform the following:

1. Retrieve the latest source code.
2. Execute the Maven build.
3. Package the application.
4. Build the Docker image.
5. Authenticate with the Docker registry.
6. Publish the image to Nexus.

Review the Jenkins console output to verify each stage.

---

# Verification Checklist

Confirm the following:

- AWS EC2 instance is running.
- Jenkins is accessible.
- Git is installed.
- Java is installed.
- Maven is installed.
- Docker is installed and operational.
- Nexus container is running.
- Jenkins credentials are configured.
- GitHub repository is reachable.
- Maven build completes successfully.
- Docker image builds successfully.
- Image is available in the Nexus repository.

---