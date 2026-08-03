# TROUBLESHOOTING.md

> A collection of real-world issues encountered while building this Jenkins CI/CD pipeline on AWS EC2, along with their root causes, resolutions, and lessons learned.

---

# Overview

Building a CI/CD pipeline is not only about getting a successful build—it is also about understanding failures and learning how to diagnose and resolve them.

Throughout this project, multiple issues were encountered involving Linux, Docker, Jenkins, Maven, Nexus Repository, GitHub integration, and networking.

Each issue below includes:

- Symptoms
- Error message
- Root cause
- Resolution
- Prevention
- Key lesson

---

# Table of Contents

1. Vim Permission Error
2. Docker Permission Denied
3. `/bin/bash` Not Found
4. Jenkins Maven Build Issues
5. Docker Registry Connection Timeout
6. Docker Registry 404 Error
7. Docker Login Empty Password
8. Docker Registry Connection Refused
9. Docker Registry 401 Unauthorized
10. Docker Port Mapping
11. Jenkins Credentials
12. General Debugging Strategy

---

# Issue 1: Vim Could Not Save File

## Error

```text
E212: Can't open file for writing
```

---

## Symptoms

Attempting to save changes using:

```vim
:wq
```

or

```vim
:wq!
```

resulted in an error.

---

## Root Cause

The current user did not have write permission for the file or directory.

This commonly occurs when editing system files without elevated privileges.

---

## Resolution

Open the file with elevated privileges when appropriate:

```bash
sudo vim <filename>
```

Alternatively, verify file ownership and permissions:

```bash
ls -l <filename>
```

Adjust permissions only when necessary and with care.

---

## Lesson Learned

Always verify file permissions before editing protected system files.

---

# Issue 2: Docker Permission Denied

## Error

```text
permission denied while trying to connect to the Docker daemon socket
```

---

## Root Cause

The current user was not permitted to communicate with the Docker daemon.

---

## Resolution

Run the command with administrative privileges:

```bash
sudo docker ...
```

Or add the user to the Docker group:

```bash
sudo usermod -aG docker $USER
```

Then sign out and sign back in for the new group membership to take effect.

---

## Lesson Learned

Understanding Linux user permissions is essential when working with Docker.

---

# Issue 3: /bin/bash Not Found

## Error

```text
exec: "/bin/bash": no such file or directory
```

---

## Root Cause

The container image was based on Alpine Linux, which typically includes `sh` instead of `bash`.

---

## Resolution

Use:

```bash
docker exec -it <container> sh
```

instead of:

```bash
docker exec -it <container> /bin/bash
```

---

## Lesson Learned

Do not assume every container includes Bash. Check the base image and use the shell it provides.

---

# Issue 4: Docker Registry Timeout

## Error

```text
Client.Timeout exceeded while awaiting headers
```

---

## Root Cause

Jenkins could not establish a connection to the Docker Registry.

Possible causes included:

- Closed security group ports
- Registry not running
- Incorrect registry port
- Network connectivity issues

---

## Resolution

Verified:

- EC2 Security Group rules
- Nexus container status
- Docker port mappings
- Registry endpoint

---

## Lesson Learned

When troubleshooting connectivity, first confirm that the target service is actually running and listening on the expected port.

---

# Issue 5: HTTP 404 Not Found

## Error

```text
404 Not Found
```

during:

```bash
docker login
```

---

## Root Cause

The request was sent to an endpoint that was not configured as a Docker Registry.

---

## Resolution

Confirmed the correct Docker hosted repository configuration inside Nexus and verified the registry endpoint.

---

## Lesson Learned

A web interface being available does not automatically mean the Docker Registry endpoint is correctly configured.

---

# Issue 6: Empty Password

## Error

```text
password is empty
```

---

## Root Cause

The password supplied to `docker login --password-stdin` was empty because Jenkins did not provide the expected credential.

---

## Resolution

Reviewed the Jenkins Credentials configuration and ensured the correct credential was referenced by the pipeline.

---

## Lesson Learned

Before assuming an authentication issue, verify that the pipeline is actually receiving the expected credentials.

---

# Issue 7: Connection Refused

## Error

```text
connect: connection refused
```

---

## Root Cause

The registry service was not listening on the requested port.

---

## Resolution

Verified:

```bash
docker ps
```

Checked listening ports:

```bash
sudo ss -tlnp
```

Restarted the Nexus container after correcting the port configuration.

---

## Lesson Learned

"Connection refused" often indicates that no service is listening on the target port.

---

# Issue 8: 401 Unauthorized

## Error

```text
401 Unauthorized
```

---

## Root Cause

Authentication with the Docker Registry failed.

Potential causes included:

- Incorrect username
- Incorrect password
- Insufficient permissions
- Registry authentication configuration

---

## Resolution

Verified the credentials stored in Jenkins and confirmed that the account had permission to access the Docker hosted repository.

---

## Lesson Learned

Authentication failures should be investigated by checking both the credentials and the registry's access controls.

---

# Issue 9 — Docker Port Mapping

## Challenge

Initially, the Nexus container exposed only the web interface.

The Docker Registry endpoint required an additional port mapping.

---

## Resolution

Started the container with the required ports:

```bash
docker run -d \
--name nexus \
-p 8081:8081 \
-p 8083:8083 \
-v nexus-data:/nexus-data \
sonatype/nexus3
```

Verified:

```bash
docker ps
```

and:

```bash
sudo ss -tlnp | grep 8083
```

---

## Lesson Learned

Exposing a container port (`-p host:container`) makes it reachable from outside the container, but the application inside the container must also be configured to use that port.

---

# Issue 10: Jenkins Credentials

## Challenge

The pipeline required access to external services such as GitHub and Nexus.

---

## Resolution

Configured credentials in:

```
Manage Jenkins
→ Credentials
```

Referenced the credentials within Jenkins jobs and pipelines rather than hardcoding usernames or passwords.

---

## Lesson Learned

Secret management is a core part of secure CI/CD design.

---

# General Debugging Strategy

A structured troubleshooting approach was followed throughout the project:

1. Read the complete error message.
2. Identify the failing stage.
3. Check service status.
4. Verify network connectivity.
5. Review logs.
6. Confirm configuration.
7. Test changes incrementally.
8. Re-run the pipeline after each fix.

Avoid making multiple changes at once. Isolating one variable at a time makes it easier to identify the true cause of an issue.

---

# Key Takeaways

This project demonstrated that successful DevOps work involves much more than writing automation scripts.

Practical engineering requires:

- Reading logs carefully
- Understanding Linux fundamentals
- Knowing how services communicate
- Managing credentials securely
- Diagnosing networking issues
- Verifying assumptions with evidence
- Documenting solutions for future reference

Each issue resolved during this project strengthened both technical knowledge and troubleshooting skills.

---

# Related Documentation

- `setup.md`
- `commands.md`
- `lessons-learned.md`
- `architecture.md`