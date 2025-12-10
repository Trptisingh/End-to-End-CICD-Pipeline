# Python Flask CI/CD Pipeline with Jenkins, Docker & Docker Hub

This repository showcases a complete **end-to-end CI/CD pipeline** using **Jenkins, Docker, Docker Compose, Docker Hub, and GitHub Webhooks** to automate the build, test, containerization, and deployment of a Python Flask application.

This project reflects industry-level CI/CD workflow and is ideal for DevOps learning & real-time implementation.

---

# 📂 **Project Structure**

The following files are included in the repository (source code available in repo):

```
├── app.py
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
└── Jenkinsfile
```

---

# **What Each File Does**

### **app.py**

A simple Python Flask application that serves a basic HTTP response.
Used to demonstrate automated deployments.

### **requirements.txt**

Contains the Python modules required for the Flask application.

### **Dockerfile**

A multi-stage Dockerfile that:

* Installs dependencies
* Copies application code
* Produces a small, optimized production image

### **docker-compose.yml**

Defines how the app will be deployed in production:

* Pulls latest image from Docker Hub
* Exposes application on port 5000
* Runs with restart policy

### **Jenkinsfile**

A Declarative Jenkins Pipeline that:

* Pulls code from GitHub
* Runs tests
* Builds Docker images
* Pushes to Docker Hub
* Deploys using Docker Compose

---

#  **Prerequisites**

Before running this CI/CD project, install the following on your Jenkins master machine:

---

#  **STEP–BY–STEP SETUP & COMMANDS**

---

## **1. Install Java**

Jenkins needs Java to run.

```
sudo apt install openjdk-17-jre -y
```

---

##  **2. Install Jenkins**

```
sudo nano install.sh

sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian/jenkins.io-2023.key

echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install jenkins -y
```

---

## **3. Install Docker, Docker Compose & Python Tools**

```
sudo apt install docker.io -y
sudo apt install docker-compose -y
sudo apt install python3-pip -y
```

---

##  **4. Add Jenkins User to Docker Group**

This allows Jenkins to run Docker commands.

```
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
sudo systemctl restart docker
```

---

# **CI/CD FLOW OVERVIEW**

1. GitHub push triggers webhook
2. Jenkins Pipeline starts
3. Code is pulled
4. Tests run
5. Docker image is built
6. Image pushed to Docker Hub
7. docker-compose deploys container
8. Application accessible on port 5000

---

#  **Docker & Deployment Workflow**

### ✔ Build application image

### ✔ Tag with build number

### ✔ Tag "latest"

### ✔ Push to Docker Hub

### ✔ Pull & deploy using docker-compose

All steps are automated through Jenkinsfile.

---

#  **Jenkins Plugin Requirements**

Install the following plugins:

* **Docker Pipeline**
* **GitHub Integration Plugin**

---

#  **Add Docker Hub Credentials in Jenkins**

Path:
**Jenkins → Credentials → Global Credentials**

Add new credential:

* **ID:** `dockerhub-cred`
* **Username:** your DockerHub username
* **Password:** your DockerHub password

---

#  **GitHub Webhook Setup**

GitHub → Repository → Settings → Webhooks → **Add Webhook**

**Payload URL**

```
http://<your-jenkins-server-ip>/github-webhook/
```

**Content Type:** `application/json`
Trigger: **Just the push event**

---

#  **Jenkins Pipeline Setup**

### **Step 1 — Create a Pipeline Job**

Jenkins → New Item → Enter name
Choose **Pipeline** → OK

### **Step 2 — Load Jenkinsfile from GitHub**

Pipeline → Definition → **Pipeline script from SCM**

* SCM: **Git**
* Repository URL:
  `https://github.com/<your-repo>/jenkins-docker-python-cicd-pipeline.git`
* Credentials: GitHub token/username
* Branch:
  `*/main`

### **Save the Job**

---

#  **First Build (Manual Trigger)**

Click **Build Now**

Jenkins will:

* Pull repository
* Run tests
* Build & tag Docker image
* Push image to Docker Hub
* Deploy using Docker Compose

---

#  **Pipeline Logs—Expected Output**

During the run, you should see logs for:

✔ Git checkout
✔ Python test execution
✔ Docker build
✔ Docker push
✔ docker-compose deployment
✔ Success message

---

#  **Verify Running Containers**

After deployment:

```
sudo docker ps
```

You should see container running on port **5000**.

---

#  **Test Application**

Open your browser:

```
http://<your-server-ip>:5000
```

You should see the Flask app responding.

---

#  **What You Will Learn**

This project teaches:

* Jenkins installation
* Docker pipeline automation
* Docker Hub integration
* Multi-stage Docker builds
* GitHub webhook automation
* CI → CD end-to-end rollout
* Writing enterprise-grade Jenkins Pipelines

