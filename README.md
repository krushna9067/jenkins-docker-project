# 🚀 Automated CI/CD Using Jenkins and Docker on AWS EC2

## 📌 Project Overview
This project demonstrates how to build a fully automated **CI/CD pipeline** using **Jenkins and Docker** on an **AWS EC2** instance.  
Whenever code is pushed to GitHub, Jenkins automatically builds a Docker image and deploys the application.

---

## 🏗️ Architecture
```

Developer → GitHub → Jenkins → Docker → AWS EC2

````

---

## 🛠️ Technologies Used
- AWS EC2 (Ubuntu 20.04)
- Jenkins
- Docker
- Git & GitHub
- Python (Flask)

---

## ⚙️ Setup Steps

### Step 1: Launch AWS EC2
- Launch Ubuntu EC2 instance
- Open security group ports:
  - 22 (SSH)
  - 8080 (Jenkins)
  - 5000 (Application)

---

### Step 2: Install Jenkins
```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
sudo apt install jenkins -y
sudo systemctl start jenkins
````

Access Jenkins:

```
http://<EC2-PUBLIC-IP>:8080
```

---

### Step 3: Install Docker

```bash
sudo apt install docker.io -y
sudo systemctl start docker
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

---

### Step 4: Application Code

#### app.py

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "Automated CI/CD using Jenkins and Docker on AWS EC2"

app.run(host="0.0.0.0", port=5000)
```

#### requirements.txt

```
flask
```

---

### Step 5: Dockerfile

```Dockerfile
FROM python:3.10
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```

---

### Step 6: Jenkins Pipeline (Jenkinsfile)

```groovy
pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your-username/your-repo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t cicd-docker-app .'
            }
        }
        stage('Run Docker Container') {
            steps {
                sh '''
                docker stop app || true
                docker rm app || true
                docker run -d -p 5000:5000 --name app cicd-docker-app
                '''
            }
        }
    }
}
```

---

## 🔁 CI/CD Workflow

1. Code is pushed to GitHub
2. Jenkins pipeline is triggered
3. Docker image is built
4. Docker container is deployed on EC2
5. Application is live

---

## 🌐 Application URL

```
http://<EC2-PUBLIC-IP>:5000
```

---

## ✅ Outcome

* Automated build and deployment
* Dockerized application
* Jenkins CI/CD pipeline
* Hands-on DevOps experience

---

## 👨‍💻 Author

**Krushna Nangare**
DevOps Engineer 

---

## ⭐ Conclusion

This project showcases a real-world CI/CD implementation using Jenkins and Docker on AWS EC2.
