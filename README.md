# 🚀 CI/CD Pipeline with Jenkins on WSL for Python App Deployment

This project demonstrates how to set up a **CI/CD pipeline** using **Jenkins** to automatically deploy a Python application from a GitHub repository.  

---

## 📂 Project Overview
- **Source Code**: Hosted on GitHub
- **CI/CD Tool**: Jenkins running inside WSL (Ubuntu)
- **Deployment Target**: Local WSL (simulating EC2)
- **Pipeline Trigger**: 
  - SCM Polling (works locally without webhooks)
  - OR GitHub Webhook via ngrok/localtunnel (optional)
- **App**: Simple Python Flask web application

---

## Install Jenkins

```shell
sudo apt update && sudo apt upgrade -y
sudo apt install openjdk-11-jdk -y
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
/usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```


Access Jenkins → http://localhost:8080

## Install Git

```shell
sudo apt install git -y
```


## Install Python

```shell
sudo apt install python3 python3-pip -y
```

## Jenkins Job Setup

Open Jenkins → http://localhost:8080

Create New Item → Select Freestyle Project

Configure GitHub Repository:

Source Code Management → Git → Add your repo URL

## Configure Build Steps:

Dockerfile

```shell
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```

## Example Python App

app.py

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "Hello from Jenkins CI/CD Pipeline!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```


requirements.txt

```txt
flask
```

## Run manually for testing:

```shell
pip3 install flask
python3 app.py
```

Check at → http://public-ip:5000

## How CI/CD Works

Developer pushes code to GitHub

Jenkins detects changes (polling/webhook)

Jenkins pulls latest code

Jenkins installs dependencies

Jenkins deploys (runs Flask app in background)

## Webhook Secret (Optional)

If using GitHub Webhook:

Set a secret token in GitHub → Jenkins GitHub Plugin

Ensures payloads are trusted & secure

## Outcome

This project demonstrates a complete CI/CD pipeline for Python app deployment with Jenkins on Linux, simulating real-world workflows.
