# VIDEO-SCRIPT.md

> A walkthrough script for demonstrating the Jenkins CI Pipeline project.

---

# Video Overview

**Project Title**

Building a Jenkins Continuous Integration (CI) Pipeline with Java, Maven, Docker, Nexus Repository, and AWS EC2

**Estimated Duration**

8–12 Minutes

---

# Opening (0:00 – 0:45)

Hello everyone.

My name is Chukwuemeka Peter Eze, and in this project I'll be demonstrating how I built a complete Jenkins Continuous Integration pipeline for a Java Maven application.

The entire project was implemented on an AWS EC2 Ubuntu server using several DevOps tools, including:

- Jenkins
- GitHub
- Java
- Maven
- Docker
- Sonatype Nexus Repository
- Linux (Ubuntu)

The objective of this project was to automate the software build process, package the application into a Docker image, and publish that image to a private Docker registry hosted on Nexus Repository.

---

# Project Architecture (0:45 – 2:00)

(Display your architecture diagram.)

Before looking at the implementation, let's understand the overall architecture.

The workflow begins when source code is stored in GitHub.

Jenkins connects to GitHub and checks out the latest version of the application.

After retrieving the code, Jenkins executes a Maven build to compile and package the application into a JAR file.

Once the build succeeds, Docker creates a container image using the generated JAR.

The pipeline then authenticates with a Docker hosted repository in Sonatype Nexus Repository and pushes the Docker image there.

This creates a repeatable and automated build process that can be executed whenever changes are made to the application.

---

# AWS Infrastructure (2:00 – 3:00)

(Display the AWS EC2 console and security group.)

Everything in this project runs on an Ubuntu EC2 instance hosted on AWS.

The instance hosts:

- Jenkins
- Docker
- Nexus Repository
- Java
- Maven

I also configured the required security group rules to allow access to:

- SSH
- Jenkins
- Nexus web interface
- Docker Registry endpoint

This environment closely resembles the type of infrastructure used in many real-world development environments.

---

# GitHub Repository (3:00 – 4:00)

(Display the GitHub repository.)

The repository contains both the application source code and all supporting documentation.

Key files include:

- `Jenkinsfile`
- `Dockerfile`
- `pom.xml`
- Project documentation
- Troubleshooting guide
- Commands reference

Keeping both the application and pipeline configuration under version control makes the project easier to maintain and reproduce.

---

# Jenkins Pipeline (4:00 – 5:30)

(Display the Jenkins dashboard and pipeline/job configuration.)

Inside Jenkins, I configured a job that performs the following steps:

1. Clone the application from GitHub.
2. Execute a Maven package build.
3. Generate the application JAR.
4. Build a Docker image.
5. Authenticate with Nexus Repository.
6. Push the Docker image to the private Docker registry.

Each build produces detailed logs, making it easy to identify and troubleshoot issues.

This project demonstrates how Jenkins automates repetitive software delivery tasks.

---

# Maven Build (5:30 – 6:15)

(Display the successful Maven build logs.)

Maven handles dependency management and project packaging.

Running the package phase compiles the Java application and generates a deployable JAR file.

Automating this process with Jenkins ensures every build follows the same standardized workflow.

---

# Docker Build (6:15 – 7:00)

(Display the Dockerfile and `docker images` output.)

After the Maven build completes successfully, Docker packages the application into a container image.

Containerization improves consistency by ensuring the application runs in the same environment regardless of where it is deployed.

---

# Nexus Repository (7:00 – 8:00)

(Display the Nexus Repository interface.)

Rather than storing Docker images on a public registry, this project uses a private Docker hosted repository in Sonatype Nexus Repository.

Using a private registry provides:

- Better control over internal artifacts.
- Version management.
- Secure image storage.
- Centralized distribution for deployment pipelines.

---

# Challenges Encountered (8:00 – 10:00)

(Display screenshots of Jenkins console output and terminal errors.)

Like most engineering projects, this implementation involved several challenges.

Some of the issues I encountered included:

- Docker permission errors.
- Using `/bin/bash` in Alpine-based containers.
- Nexus Docker Registry configuration.
- Registry authentication failures.
- Docker port mapping issues.
- Connection refused errors.
- HTTP versus HTTPS registry configuration.
- Jenkins credential configuration.

Rather than making random changes, I followed a structured debugging process:

- Read the error carefully.
- Identify the failing component.
- Verify logs.
- Confirm configuration.
- Test one change at a time.

This systematic approach helped isolate and resolve each issue while improving my troubleshooting skills.

---

# Key Lessons (10:00 – 11:00)

This project reinforced several important engineering concepts.

I gained practical experience with:

- Continuous Integration.
- Pipeline as Code.
- Jenkins automation.
- Maven build lifecycle.
- Docker containerization.
- Artifact management.
- Linux administration.
- AWS infrastructure.
- Networking.
- Technical troubleshooting.

Beyond learning the tools themselves, I developed a stronger understanding of how they integrate into a complete software delivery workflow.

---

# Closing (11:00 – 12:00)

Thank you for watching this project walkthrough.

This repository includes detailed documentation covering:

- Project setup.
- Architecture.
- Commands used.
- Troubleshooting.
- Lessons learned.
- Project structure.

If you're interested in discussing DevOps, Cloud Engineering, CI/CD, or Infrastructure Automation, feel free to connect with me on LinkedIn or explore the repository on GitHub.

Thank you for your time.

---

# Recommended Screen Recording Flow

Record in this order:

1. Introduction slide.
2. Architecture diagram.
3. AWS EC2 console.
4. GitHub repository.
5. Jenkins dashboard.
6. Jenkins job configuration.
7. Successful build history.
8. Console Output.
9. `pom.xml`
10. `Dockerfile`
11. Docker images.
12. Nexus Repository.
13. Troubleshooting documentation.
14. Lessons learned.
15. Final repository overview.

---

# Recommended On-Screen Assets

Prepare these visuals before recording:

- Repository homepage.
- Architecture diagram.
- AWS EC2 dashboard.
- Security Group configuration.
- SSH terminal.
- Jenkins dashboard.
- Jenkins build history.
- Successful console output.
- `Jenkinsfile`.
- `Dockerfile`.
- `pom.xml`.
- `docker images`.
- `docker ps`.
- Nexus Repository dashboard.
- Docker hosted repository.
- Uploaded Docker image.
- README documentation.
- Troubleshooting guide.

---

# Presentation Tips

- Speak naturally rather than reading word-for-word.
- Explain *why* each tool is used, not just *what* it does.
- Keep terminal windows zoomed for readability.
- Highlight how the tools work together as one system.
- If discussing a problem, explain both the cause and the solution.
- End with a brief summary of what you learned and what you plan to build next.
