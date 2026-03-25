# Jenkins + Docker CI/CD Web Deployment

## Project Overview

This project shows a **simple DevOps CI/CD pipeline** using:

* **GitHub** – stores the code
* **Jenkins** – automation server that runs the pipeline
* **Docker** – builds and runs the website container
* **Nginx** – serves the website

Whenever code changes are pushed to GitHub, Jenkins builds a new Docker image and deploys the updated website automatically.

---

# Architecture

```
Developer
   ↓
Git Push (GitHub)
   ↓
Jenkins Pipeline
   ↓
Docker Build
   ↓
Stop Old Container
   ↓
Run New Container
   ↓
Website deployed on localhost:8082
```

---

# Project Structure

```
jenkins-docker-cicd/
│
├── Dockerfile
├── Jenkinsfile
├── index.html
└── README.md
```

---

# Step 1 — Install Required Tools

Install the following tools:

* Git
* Docker Desktop
* Jenkins
* Java (JDK 17)

Check installations:

```
git --version
docker --version
java -version
```

---

# Step 2 — Create Simple Website

Create an `index.html` file.

```
<!DOCTYPE html>
<html>
<head>
<title>DevOps CI/CD Project</title>
</head>
<body>
<h1>Hello from my Jenkins Docker Pipeline 🚀</h1>
</body>
</html>
```

---

# Step 3 — Create Dockerfile

Create a `Dockerfile`.

```
FROM nginx:latest
COPY . /usr/share/nginx/html
```

Explanation:

* `FROM nginx:latest` → uses nginx image
* `COPY . /usr/share/nginx/html` → copies website files into nginx server

---

# Step 4 — Test Docker Locally

Build the Docker image.

```
docker build -t devops-web .
```

Run container:

```
docker run -d -p 8082:80 devops-web
```

Open browser:

```
http://localhost:8082
```

Your website should load.

---

# Step 5 — Push Project to GitHub

Initialize Git.

```
git init
git add .
git commit -m "Initial commit"
```

Add remote repository.

```
git remote add origin https://github.com/YOUR_USERNAME/jenkins-docker-cicd.git
```

Push code.

```
git push -u origin main
```

---

# Step 6 — Create Jenkins Pipeline Job

Open Jenkins:

```
http://localhost:8080
```

Create new job.

```
New Item → Pipeline
```

Name:

```
docker-web-cicd
```

---

# Step 7 — Connect Jenkins to GitHub

In pipeline configuration:

```
Pipeline → Pipeline script from SCM
```

Select:

```
Git
```

Repository URL:

```
https://github.com/YOUR_USERNAME/jenkins-docker-cicd.git
```

Branch:

```
main
```

Script path:

```
Jenkinsfile
```

---

# Step 8 — Create Jenkinsfile

Create a `Jenkinsfile`.

```
pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t devops-web .'
            }
        }

        stage('Stop Old Container') {
            steps {
                bat 'docker stop devops-container || exit 0'
                bat 'docker rm devops-container || exit 0'
            }
        }

        stage('Deploy Container') {
            steps {
                bat 'docker run -d -p 8082:80 --name devops-container devops-web'
            }
        }

    }
}
```

Explanation:

| Stage              | Purpose                            |
| ------------------ | ---------------------------------- |
| Build Docker Image | Builds the website image           |
| Stop Old Container | Removes previous running container |
| Deploy Container   | Runs new container                 |

---

# Step 9 — Run Jenkins Pipeline

In Jenkins click:

```
Build Now
```

Pipeline will:

1. Pull code from GitHub
2. Build Docker image
3. Stop old container
4. Deploy new container

---

# Step 10 — Access Website

Open browser:

```
http://localhost:8082
```

Your website is now deployed through Jenkins.

---

# Step 11 — Automatic Builds (Polling)

Since GitHub cannot reach local Jenkins directly, we enabled **SCM Polling**.

Go to:

```
Jenkins → Job → Configure
```

Enable:

```
Poll SCM
```

Schedule:

```
H/2 * * * *
```

Meaning:

```
Jenkins checks GitHub every 2 minutes for new changes
```

If new code is found, Jenkins automatically runs the pipeline.

---

# CI/CD Flow

```
Edit code
   ↓
git push
   ↓
Jenkins checks GitHub
   ↓
Pipeline runs
   ↓
Docker builds image
   ↓
Container deployed
   ↓
Website updated
```

---

# Technologies Used

* Git
* GitHub
* Jenkins
* Docker
* Nginx
* CI/CD Pipeline

---

# Learning Outcome

From this project we learned:

* How CI/CD pipelines work
* How Jenkins automates builds
* How Docker containers deploy applications
* How to integrate GitHub with Jenkins
* How automated deployment works in DevOps

---

# Future Improvements

Possible upgrades:

* Push Docker images to DockerHub
* Deploy to cloud server (AWS / Azure)
* Use Kubernetes for container orchestration
* Add automated testing stage

---

# Author

**Priyanka J L**

DevOps beginner project for learning CI/CD automation with Jenkins and Docker.
