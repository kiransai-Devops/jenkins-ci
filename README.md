# 🚀 Jenkins CI Pipeline with AWS ECR & Trivy

## 📌 Project Overview

This project demonstrates an end-to-end Continuous Integration (CI) pipeline using Jenkins with a Master-Agent architecture.

The pipeline automates:

* Code checkout from GitHub
* Docker image build using Jenkins
* Security scanning using Trivy
* Pushing images to AWS Elastic Container Registry (ECR) via Jenkins

---

## 🏗️ Architecture

* Jenkins Master (EC2)
* Jenkins Agent (EC2)
* GitHub Repository
* Docker
* AWS ECR
* Trivy (Security Scanner)

---

## ⚙️ Tech Stack

* **CI Tool:** Jenkins
* **Cloud:** AWS (EC2, ECR)
* **Containerization:** Docker
* **Security Tool:** Trivy
* **Version Control:** GitHub
* **Operating System:** Linux (Amazon Linux / Ubuntu)

---

## 🔧 Setup Instructions

### 1. Create EC2 Instances

* Launch 2 EC2 instances:

  * Jenkins Master
  * Jenkins Agent
* Instance Type: `t3.micro`
  <img width="1416" height="311" alt="Screenshot 2026-04-04 153205" src="https://github.com/user-attachments/assets/6778960f-4b5a-4e4e-9a75-bea382e2c589" />


---

### 2. Install Jenkins (Master)

```bash id="a1"
sudo curl -L -o /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
# Add required dependencies for the jenkins package
sudo yum install fontconfig java-21-openjdk -y
sudo yum install jenkins -y
sudo systemctl daemon-reload
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

Access Jenkins:

```
http://jenkins.kidevops.shop:8080
```

Get initial admin password:

```bash id="a2"
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

### 3. Configure Jenkins

* Install suggested plugins
* Create admin user
  <img width="1490" height="810" alt="Screenshot 2026-04-04 205533" src="https://github.com/user-attachments/assets/136f2e7a-e232-4705-81d8-4f40dd86868a" />


Required plugins:

* Pipeline Stage View
* AWS Credentials
* Pipeline AWS Steps
  <img width="1791" height="392" alt="Screenshot 2026-04-04 213834" src="https://github.com/user-attachments/assets/b0f14751-55b9-44b6-8200-41c043e7eeb8" />
  <img width="1457" height="581" alt="Screenshot 2026-04-04 214032" src="https://github.com/user-attachments/assets/4cdc351d-21e3-4083-807e-f5c73500b166" />


---

### 4. Setup Jenkins Agent

* Install Java on agent

Configure node:

* Name: `Agent-1`
* Remote Directory: `/home/ec2-user/jenkins`
* Launch Method: SSH

<img width="1600" height="721" alt="Screenshot 2026-04-04 221133" src="https://github.com/user-attachments/assets/18556d19-27c1-491b-8e00-e86f7d65999e" />

<img width="1697" height="410" alt="Screenshot 2026-04-04 221307" src="https://github.com/user-attachments/assets/2173e0bd-3fa2-4e54-8291-e577052e9ea3" />



Add credentials:

* Username: `ec2-user`
* SSH Key / Password

  <img width="1341" height="847" alt="Screenshot 2026-04-04 214513" src="https://github.com/user-attachments/assets/68bed80e-844b-477e-82a3-ae1d2867026d" />


---

### 5. GitHub Integration

```bash id="a3"
git clone https://github.com/kiransai-Devops/jenkins-ci.git
```

---

### 6. Configure AWS ECR

* Create ECR repository:

```
roboshop/catalogue
```

<img width="1837" height="332" alt="Screenshot 2026-04-05 095032" src="https://github.com/user-attachments/assets/7dd62dd5-4ca0-4e09-99f7-1b0dc0e683e8" />


* Store AWS credentials in Jenkins:

```
Manage Jenkins → Credentials
```

Add:

* AWS Access Key
* AWS Secret Key
<img width="742" height="812" alt="Screenshot 2026-04-05 100236" src="https://github.com/user-attachments/assets/0bb34876-9315-4221-8762-643e8033e3cf" />

---

## 🔄 CI Pipeline Stages

1. Checkout Code
2. Read Version
3. Install Dependencies
4. Build Docker Image (via Jenkins agent)
5. Trivy Security Scan
6. Authenticate to AWS ECR
7. Tag Docker Image
8. Push Image to AWS ECR
9. Post Actions

---

## 🔐 Security Scanning (Trivy)

Trivy is integrated into the Jenkins pipeline to scan Docker images.

**Sample Result:**

* Medium: 21
* High: 4
* Critical: 2

<img width="848" height="122" alt="Screenshot 2026-04-05 142409" src="https://github.com/user-attachments/assets/ba171290-476d-4127-8d18-cd4066c01d19" />

---

## 📊 Pipeline Outcome

* Fully automated CI pipeline
* Docker build and push handled inside Jenkins
* Secure image scanning before deployment
* Distributed build using Jenkins agent
* Successful push to AWS ECR

<img width="1555" height="395" alt="Screenshot 2026-04-05 142939" src="https://github.com/user-attachments/assets/6aec73e9-ffcc-4e87-9d43-86e819732bcd" />

---

---

## ✅ Key Features

* Jenkins Master-Agent architecture
* Fully automated CI pipeline
* Docker image build inside Jenkins
* Automated push to AWS ECR
* Integrated security scanning using Trivy
* Secure credential management

---

## 🚨 Challenges Faced

* Jenkins agent SSH connectivity issues
* Plugin dependency conflicts
* AWS credential configuration errors
* Handling Trivy scan failures

---
Note on SonarQube

I have hands-on experience with SonarQube, including:

Setting up SonarQube server
Creating and configuring Quality Gates
Generating and managing authentication tokens
Integrating SonarQube with Jenkins pipelines
Configuring webhooks for pipeline feedback

Due to the lack of a free-tier managed SonarQube service in AWS, it was not included in this project implementation. However, I am comfortable integrating SonarQube into CI/CD pipelines in real-world scenarios.

