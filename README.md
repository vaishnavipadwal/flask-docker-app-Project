# Flask Docker App Deployment - Assignment with Report

## Assignment: 
- Create a Git repository (local) with the code at
https://flask.palletsprojects.com/en/stable/quickstart/#a-minimal-application

- Write scripts to build a Docker image
- Deploy the docker image on the local machine in a docker container
- Tag the git branch with a “release” tag
- Make a new branch, change the code to say “Hello new world”, commit and push the code.
- Make a docker image for the code at the release tag.
- Deployment of Docker Image to AWS Fargate / EC2 to test the application. Alternatively, deploy to local Docker installation, with Caddy or Traefik as the reverse proxy, and SSL termination to handle https traffic.

Share the Git repository so that we can review the process followed

## Report:

This assignment involved creating a minimal Flask application, containerizing it using Docker, managing code versions with Git, and deploying the Docker image locally and on AWS EC2. The key tasks were building and tagging Docker images, managing Git branches and tags, and deploying the app to test environments.

## Step 1: Create Local Git Repository with Flask Code

- Cloned the minimal Flask application example from the official Flask documentation:  
  https://flask.palletsprojects.com/en/stable/quickstart/#a-minimal-application

- Initialized a local Git repository and committed the initial Flask app code.


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

  ### Result:
  ![resultonlocalmachine](resultonlocalmachine.png)

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
      return "Hello world from Vaishnavi"
  ```
  ### Result:
  ![updatedresultonlocalmachine](updatedresultonlocalmachine.png)
  
* Committed and pushed the new branch:

  ```bash
  git add app.py
  git commit -m "Updated greeting to Hello world from Vaishnavi"
  git push -u origin new-feature
  ```

## Step 6: Build Docker Image for Release Tag

* Checked out the release tag locally:

  ```bash
  git checkout tags/release
  ```

* Built a Docker image for the release version:

  ```bash
  docker build -t flask-app:release .
  ```


## Step 7: Deploy Docker Image on AWS EC2

* Launched an Ubuntu EC2 instance with security group allowing port 5000.

* Installed Docker on EC2 instance.

* Pulled the image from Docker Hub or built image on EC2.

* Ran the container exposing port 5000:

  ```bash
  docker run -d -p 5000:5000 --name flask-container flask-app:release
  ```

* Verified access to the Flask app at `http://<ec2-public-ip>:5000`.

  ### Result:
  ![ec2result](ec2result.png)
  

## GitHub Repository

The complete source code and Git history can be reviewed at:
[https://github.com/vaishnavipadwal/flask-docker-app-Project](https://github.com/vaishnavipadwal/flask-docker-app-Project)

## Dockerhub link for image
[https://hub.docker.com/repository/docker/vaishnavipadwal/flask-app/general](https://hub.docker.com/repository/docker/vaishnavipadwal/flask-app/general)
# Conclusion

This assignment demonstrated practical skills in Dockerizing a Python Flask app, managing source control with Git, and deploying containerized applications both locally and on AWS EC2. Key DevOps concepts like tagging releases, branching, container deployment, and logging were applied successfully.
