**Jenkins Project - CI/CD Pipeline**

**Overview**
This project demonstrates a simple Jenkins pipeline to automate building, testing, and deploying a Dockerized application. The pipeline uses Jenkins + Docker to achieve continuous integration and delivery.

**Pipeline Stages**
**Build**: Builds Docker image jenkins-project:latest

**Test**: Runs basic tests (customize as needed)

**Deploy**: Stops previous container (if exists) and runs new one


🔑 **Issues You Faced**

**Plugin installation errors**
Errors like No such plugin: gradle or No such plugin: cloudbees-folder.

Cause: You tried to install non-existent or mis-typed plugins. Jenkins requires correct plugin names from the update center.

**Docker pipeline error**
Error: No such property: docker for class: groovy.lang.Binding.

Cause: Using docker.build DSL without enabling Docker Pipeline plugin.

Fix: Switched to sh 'docker build …'.

Main blocker — docker: not found

**Jenkins container could not run Docker commands.**
Cause: Jenkins image does not ship with Docker CLI by default.

Every pipeline run failed at docker build -t jenkins-project:latest ..

Tried to exec into Jenkins container

Found docker: command not found inside container.

Attempted apt-get install docker.io but got permission denied because you were logged in as jenkins user, not root.

Missing right Jenkins image

Tried pulling jenkins/jenkins:lts-docker → got not found.

The correct image is jenkins/jenkins:lts plus manual Docker CLI install.

Installed Docker inside Jenkins container

**Fixed with:**
docker exec -it --user root jenkins bash
apt-get update && apt-get install -y docker.io


**Verified with docker --version.**
Volume/socket mapping confusion

Even with Docker installed, Jenkins must talk to host Docker daemon.

Fix: Mounted /var/run/docker.sock into Jenkins container.

✅ **Lessons You Learned**

Jenkins doesn’t ship with Docker → you must add it or run Docker-in-Docker.

Always run Jenkins with -v /var/run/docker.sock:/var/run/docker.sock so it can use host Docker.

GitHub webhooks don’t work with plain localhost. You need ngrok or a cloud VM.

docker.build requires the Docker Pipeline plugin; otherwise, just use sh "docker build …".

Always run apt-get install as root, not as the default jenkins user.

⚡ **In short:**
Your main blocker was Jenkins couldn’t find Docker, because you were running the vanilla Jenkins container without Docker CLI or socket. Once you fixed that with root + socket mount, the pipeline started behaving.


<img width="1902" height="953" alt="image" src="https://github.com/user-attachments/assets/3b25fbfd-3d26-44e6-84c5-6f8800a02802" />


<img width="1906" height="957" alt="image" src="https://github.com/user-attachments/assets/73384449-1065-439c-84c7-87f5cfd72cb2" />

