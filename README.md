# Jenkins Node.js CI/CD Pipeline

This project demonstrates a complete CI/CD pipeline setup using **Jenkins** for a simple **Node.js** application. It includes provisioning an EC2 instance, installing Jenkins, configuring a pipeline, running automated tests with **Jest**, and deploying the application.

---

## 🔧 Prerequisites

- An AWS account
- A GitHub repository (e.g., https://github.com/atubak400/jenkins-nodejs-cicd)
- Local machine with terminal access and a code editor (e.g., VS Code)

---

## 🚀 Step-by-Step Setup

### 1. Launch EC2 Instance

- Region: us-east-1 (N. Virginia)
- AMI: Ubuntu 24.04
- Instance type: t2.micro (Free Tier)
- Key pair: `jenkins-key.pem`
- Security group (open these ports):
  - SSH (TCP 22, source: 0.0.0.0/0)
  - HTTP (TCP 8080, source: 0.0.0.0/0)
  - Node.js app (TCP 3000, source: 0.0.0.0/0)

### 2. Connect via SSH
```bash
chmod 400 jenkins-key.pem
ssh -i jenkins-key.pem ubuntu@<public-ip>
```

### 3. Install Jenkins
```bash
sudo apt update
sudo apt install -y openjdk-17-jdk
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install -y jenkins
sudo systemctl start jenkins
```

### 4. Access Jenkins
- Visit: `http://<public-ip>:8080`
- Get admin password:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
- Install suggested plugins
- Create first admin user

---

## 🧪 Project Structure

```
jenkins-nodejs-cicd/
├── __tests__/
│   └── app.test.js
├── index.js
├── package.json
├── package-lock.json
├── Jenkinsfile
└── README.md
```

### `index.js`
```js
const express = require('express');
const app = express();
const PORT = 3000;

app.get('/', (req, res) => {
  res.send('Hello from Node.js CI/CD!');
});

app.listen(PORT, '0.0.0.0', () => {
  console.log(`Server running on port ${PORT}`);
});
```

### `package.json`
```json
{
  "name": "jenkins-nodejs-cicd-demo",
  "version": "1.0.0",
  "description": "A simple Node.js app with Jenkins CI/CD",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "jest": "^29.0.0"
  }
}
```

### `__tests__/app.test.js`
```js
test('sample test', () => {
  expect(1 + 1).toBe(2);
});
```

---

## ⚙️ Jenkinsfile

This defines the CI/CD pipeline:

```groovy
pipeline {
  agent any

  stages {
    stage('Clone Code') {
      steps {
        git branch: 'main', url: 'https://github.com/atubak400/jenkins-nodejs-cicd'
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        sh 'npm test'
      }
    }

    stage('Build') {
      steps {
        sh 'echo "Build step - nothing to build"'
      }
    }

    stage('Deploy') {
      steps {
        sh 'nohup node index.js > ~/node-output.log 2>&1 &'
      }
    }
  }
}
```

---

## ✅ Final Testing

Visit: `http://<public-ip>:3000`

You should see:
```
Hello from Node.js CI/CD!
```

---

## 🔁 Optional: GitHub Webhook for Auto Trigger

- In Jenkins:
  - Go to: Configure → Build Triggers
  - Enable: GitHub hook trigger for GITScm polling

- On GitHub repo:
  - Settings → Webhooks → Add Webhook
  - Payload URL: `http://<public-ip>:8080/github-webhook/`
  - Content type: `application/json`
  - Event: Just the push event
  - Disable SSL verification if using HTTP

---

## 🎉 Summary

This project covered:
- Provisioning EC2
- Installing Jenkins
- Configuring a pipeline
- Running Jest tests
- Deploying a Node.js app
- Accessing the app publicly

You can showcase this in interviews to demonstrate hands-on experience with DevOps concepts like automation, CI/CD, testing, and deployment.

---

## 🔮 Next Steps

- Use PM2 or systemd for process monitoring
- Integrate Slack/email for build notifications
- Add Docker support
- Move secrets to AWS Secrets Manager

---

**Author**: Kingsley Atuba  
**Repo**: https://github.com/atubak400/jenkins-nodejs-cicd
