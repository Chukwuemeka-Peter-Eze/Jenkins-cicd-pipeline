# COMMANDS.md

> A comprehensive reference of the commands used throughout this Jenkins CI/CD project.

---

# Overview

This document contains the Linux, Git, Docker, Maven, Jenkins, and Nexus commands used while building the CI pipeline on an AWS EC2 Ubuntu Server.

Commands are grouped by technology for easier navigation.

---

# Table of Contents

1. Linux Commands
2. Git Commands
3. Java Commands
4. Maven Commands
5. Docker Commands
6. Jenkins Commands
7. Nexus Commands
8. Networking Commands
9. Troubleshooting Commands

---

# Linux Commands

## Display Current Directory

```bash
pwd
```

Displays the current working directory.

---

## List Files

```bash
ls
```

Lists files and directories.

---

## Detailed List

```bash
ls -la
```

Displays hidden files, permissions, ownership, and sizes.

---

## Change Directory

```bash
cd directory-name
```

Moves into another directory.

---

## Create Directory

```bash
mkdir directory-name
```

Creates a new directory.

---

## Remove Directory

```bash
rm -rf directory-name
```

Deletes a directory and all of its contents.

> Use with caution. This command permanently removes files.

---

## Create Empty File

```bash
touch filename
```

Creates a new empty file.

---

## View File Contents

```bash
cat filename
```

Displays the contents of a file.

---

## Edit File

```bash
vim filename
```

Opens a file in the Vim editor.

Useful Vim commands:

```text
i      Enter Insert Mode
Esc    Exit Insert Mode
:w     Save
:q     Quit
:wq    Save and Quit
:q!    Quit Without Saving
```

---

# Git Commands

## Clone Repository

```bash
git clone <repository-url>
```

Downloads a remote Git repository.

---

## Check Repository Status

```bash
git status
```

Shows tracked, modified, and untracked files.

---

## Add Files

```bash
git add .
```

Stages all modified files.

---

## Commit Changes

```bash
git commit -m "Meaningful commit message"
```

Creates a snapshot of the staged changes.

---

## Push Changes

```bash
git push origin main
```

Pushes commits to the remote repository.

---

## Pull Latest Changes

```bash
git pull origin main
```

Updates the local repository with remote changes.

---

## View Commit History

```bash
git log
```

Displays the project's commit history.

---

# Java Commands

## Verify Java Installation

```bash
java -version
```

Displays the installed Java version.

---

## Verify Java Compiler

```bash
javac -version
```

Displays the installed Java compiler version.

---

# Maven Commands

## Verify Maven

```bash
mvn -version
```

Displays Maven version information.

---

## Clean Build Artifacts

```bash
mvn clean
```

Removes the `target` directory from previous builds.

---

## Package the Application

```bash
mvn package
```

Compiles the application, runs tests (if available), and packages the project into a JAR file.

---

## Install to Local Repository

```bash
mvn install
```

Builds the project and installs the artifact into the local Maven repository.

---

# Docker Commands

## Verify Docker

```bash
docker --version
```

Displays the installed Docker version.

---

## List Running Containers

```bash
docker ps
```

Shows currently running containers.

---

## List All Containers

```bash
docker ps -a
```

Displays both running and stopped containers.

---

## List Docker Images

```bash
docker images
```

Shows locally available images.

---

## Pull an Image

```bash
docker pull image-name
```

Downloads an image from a registry.

---

## Build an Image

```bash
docker build -t image-name:tag .
```

Builds a Docker image from the Dockerfile.

---

## Run a Container

```bash
docker run image-name
```

Starts a container from an image.

---

## Run in Detached Mode

```bash
docker run -d image-name
```

Runs the container in the background.

---

## Run with Port Mapping

```bash
docker run -d -p 8081:8081 image-name
```

Maps a container port to the host.

---

## Assign a Container Name

```bash
docker run --name nexus image-name
```

Starts a container with a custom name.

---

## View Container Logs

```bash
docker logs container-name
```

Displays container logs.

---

## Follow Logs in Real Time

```bash
docker logs -f container-name
```

Streams live logs.

---

## Execute a Command in a Running Container

```bash
docker exec -it container-name sh
```

Opens a shell inside lightweight containers (such as Alpine-based images).

> **Note:** During this project, some containers did not include `/bin/bash`, so `sh` was used instead.

---

## Stop a Container

```bash
docker stop container-name
```

Stops a running container.

---

## Start a Container

```bash
docker start container-name
```

Starts an existing container.

---

## Remove a Container

```bash
docker rm container-name
```

Deletes a stopped container.

---

## Remove an Image

```bash
docker rmi image-name
```

Deletes a Docker image.

---

## Docker Login

```bash
docker login registry-url
```

Authenticates Docker with a registry.

---

## Push an Image

```bash
docker push registry/image:tag
```

Uploads an image to a registry.

---

# Jenkins Commands

Although Jenkins is primarily managed through its web interface, these commands are useful on the server.

## Check Jenkins Status

```bash
sudo systemctl status jenkins
```

---

## Start Jenkins

```bash
sudo systemctl start jenkins
```

---

## Stop Jenkins

```bash
sudo systemctl stop jenkins
```

---

## Restart Jenkins

```bash
sudo systemctl restart jenkins
```

---

## Enable Jenkins at Boot

```bash
sudo systemctl enable jenkins
```

---

## View Jenkins Logs

```bash
sudo journalctl -u jenkins
```

---

# Nexus Commands

## Run Nexus

```bash
docker run -d \
--name nexus \
-p 8081:8081 \
-p 8083:8083 \
-v nexus-data:/nexus-data \
sonatype/nexus3
```

Starts the Nexus Repository container with persistent storage.

---

## Check Nexus Container

```bash
docker ps
```

Confirms the Nexus container is running.

---

## View Nexus Logs

```bash
docker logs nexus
```

Displays startup and runtime logs.

---

# Networking Commands

## Check Listening Ports

```bash
sudo ss -tlnp
```

Lists active listening TCP ports and associated processes.

---

## Filter for a Specific Port

```bash
sudo ss -tlnp | grep 8083
```

Verifies whether a service is listening on port `8083`.

---

## View IP Address

```bash
ip addr
```

Displays network interface information.

---

# Troubleshooting Commands

## Verify Docker Permissions

```bash
groups
```

Shows the current user's group memberships.

---

## Check Docker Service

```bash
sudo systemctl status docker
```

Verifies that the Docker daemon is running.

---

## Inspect a Container

```bash
docker inspect container-name
```

Displays detailed container configuration in JSON format.

---

## Test Registry Connectivity

```bash
curl http://<registry-host>:8083/v2/
```

Checks whether the Docker Registry endpoint is reachable.

---

# Summary

These commands formed the foundation of the Jenkins CI pipeline implementation. Understanding not only **how** to run them but also **why** they are used is essential for troubleshooting, automation, and day-to-day DevOps operations.

For issues encountered while using these commands, see **`TROUBLESHOOTING.md`**.