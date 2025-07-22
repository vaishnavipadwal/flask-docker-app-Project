# Flask Docker App Deployment - Assignment Report

## Overview

This assignment involved creating a minimal Flask application, containerizing it using Docker, managing code versions with Git, and deploying the Docker image locally and on AWS EC2. The key tasks were building and tagging Docker images, managing Git branches and tags, and deploying the app to test environments.

---

## Step 1: Create Local Git Repository with Flask Code

- Cloned the minimal Flask application example from the official Flask documentation:  
  https://flask.palletsprojects.com/en/stable/quickstart/#a-minimal-application

- Initialized a local Git repository and committed the initial Flask app code.

---

## Step 2: Create Requirements File

Create a file named requirements.txt with all Python dependencies:

```

blinker==1.9.0
click==8.2.1
colorama==0.4.6
Flask==3.1.1
itsdangerous==2.2.0
Jinja2==3.1.6
MarkupSafe==3.0.2
Werkzeug==3.1.3
````

---

## Step 3: Write Dockerfile to Build Docker Image

Created a Dockerfile with the following content:

```dockerfile
FROM python:3.11-slim


WORKDIR /app

# Install dependencies needed for building packages and for pip
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    gcc \
    libffi-dev \
    libssl-dev \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["python", "app.py"]
````

---

## Step 4: Build Docker Image Locally and Run Container

* Built the image:

  ```bash
  docker build -t flask-app .
  ```

* Ran the container locally:

  ```bash
  docker run -d -p 5000:5000 --name flask-container flask-app
  ```

* Verified the app by accessing `http://localhost:5000`.

---

## Step 5: Git Branch Tagging and Version Control

* Tagged the current `main` branch with the tag `release`:

  ```bash
  git tag release
  git push origin release
  ```

* Created a new branch `new-feature`:

  ```bash
  git checkout -b new-feature
  ```

* Modified app.py to change the greeting to "Hello world from Vaishnavi`:

  ```python
  @app.route('/')
  def hello():
      return "Hello world  from Vaishnavi"
  ```

* Committed and pushed the new branch:

  ```bash
  git add app.py
  git commit -m "Updated greeting to Hello new world"
  git push -u origin new-feature
  ```

---

## Step 6: Build Docker Image for Release Tag

* Checked out the release tag locally:

  ```bash
  git checkout tags/release
  ```

* Built a Docker image for the release version:

  ```bash
  docker build -t flask-app:release .
  ```

---

## Step 7: Deploy Docker Image on AWS EC2

* Launched an Ubuntu EC2 instance with security group allowing port 5000.

* Installed Docker on EC2 instance.

* Pulled the image from Docker Hub or built image on EC2.

* Ran the container exposing port 5000:

  ```bash
  docker run -d -p 5000:5000 --name flask-container flask-app:release
  ```

* Verified access to the Flask app at `http://<ec2-public-ip>:5000`.

---

## Git Branching Strategies and Trunk Based Development

* Multiple branches (main, new-feature) were used to separate stable release code and ongoing development.

* **Trunk Based Development** involves developers committing small, frequent changes directly to a single mainline branch (trunk), avoiding long-lived feature branches to minimize integration issues.

* In this project, we used feature branches and tags for releases, but adopting trunk-based development would mean all changes are integrated frequently to the main branch with feature toggles for incomplete work.

---

## GitHub Repository

The complete source code and Git history can be reviewed at:
[https://github.com/vaishnavipadwal/flask-docker-app-Project](https://github.com/vaishnavipadwal/flask-docker-app-Project)

---
## DockerHub link for image
[https://hub.docker.com/repository/docker/vaishnavipadwal/flask-app/general](https://hub.docker.com/repository/docker/vaishnavipadwal/flask-app/general)
# Conclusion

This assignment demonstrated practical skills in Dockerizing a Python Flask app, managing source control with Git, and deploying containerized applications both locally and on AWS EC2. Key DevOps concepts like tagging releases, branching, container deployment, and logging were applied successfully.
