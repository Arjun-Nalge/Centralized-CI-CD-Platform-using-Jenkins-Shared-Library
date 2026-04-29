# Centralized CI/CD Platform using Jenkins Shared Library

---

## 📌 Project Overview

This project implements a centralized CI/CD platform using Jenkins hosted on AWS EC2. Multiple applications use a shared Jenkins library to execute standardized pipeline stages such as Build, Test, Scan, and Deploy.

---

## 🎯 Objective

* Centralize CI/CD infrastructure
* Standardize pipelines across multiple applications
* Reduce duplication and configuration inconsistencies
* Improve maintainability and scalability

---

## 🏗️ Live Architecture

```
                +----------------------+
                |     GitHub Repos     |
                |  (app-1, app-2)      |
                +----------+-----------+
                           |
                           v
                +----------------------+
                |      Jenkins         |
                |    (EC2 Instance)    |
                +----------+-----------+
                           |
            +--------------+--------------+
            |                             |
            v                             v
  +------------------+          +------------------+
  | Shared Library   |          | Pipeline Jobs    |
  | (Reusable Code)  |          | (app-1, app-2)   |
  +------------------+          +------------------+
                           |
                           v
                +----------------------+
                | Pipeline Execution   |
                | Build → Test → Scan  |
                | → Deploy             |
                +----------------------+
```

---

## 🛠️ Technologies Used

* Jenkins
* AWS EC2 (Ubuntu)
* GitHub
* Docker (optional)

---

## ⚙️ Step-by-Step Execution

### Step 1: Launch EC2 Instance

* Launch Ubuntu EC2 instance
* Open ports: 22, 8080

---

### Step 2: Install Jenkins

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y

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

Access Jenkins:

```
http://<EC2-PUBLIC-IP>:8080
```

---

### Step 3: Install Required Plugins

* Pipeline
* Git
* Docker Pipeline
* Credentials Binding
* Role-based Authorization Strategy

---

### Step 4: Configure Agent

* Use built-in node
* Set executors = 2

---

### Step 5: Create Jenkins Shared Library

#### Repository Structure

```
jenkins-shared-library/
│
├── vars/
│   ├── buildApp.groovy
│   ├── testApp.groovy
│   ├── scanApp.groovy
│   ├── deployApp.groovy
│
└── src/
```

---

### Shared Library Code

#### buildApp.groovy

```groovy
def call() {
    echo "Building application..."
    sh "echo Build stage executed"
}
```

#### testApp.groovy

```groovy
def call() {
    echo "Running tests..."
    sh "echo Test stage executed"
}
```

#### scanApp.groovy

```groovy
def call() {
    echo "Scanning code..."
    sh "echo Scan stage executed"
}
```

#### deployApp.groovy

```groovy
def call() {
    echo "Deploying application..."
    sh "echo Deploy stage executed"
}
```

---

### Step 6: Configure Shared Library in Jenkins

* Go to **Manage Jenkins → Configure System**
* Add Global Pipeline Library:

  * Name: `shared-lib`
  * Default Branch: `main` or `Master`
  * Git URL: shared library repo

---

### Step 7: Create Sample Applications

#### App Structure

```
app-1/
│
├── Jenkinsfile
└── app.js
```

---

### Sample Code

```bash
echo "Hello App 1" > app.js
```

---

### Jenkinsfile (Used in Both Apps)

```groovy
@Library('shared-lib') _

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                buildApp()
            }
        }

        stage('Test') {
            steps {
                testApp()
            }
        }

        stage('Scan') {
            steps {
                scanApp()
            }
        }

        stage('Deploy') {
            steps {
                deployApp()
            }
        }
    }
}
```

---

### Step 8: Create Jenkins Jobs

* New Item → Pipeline
* Select **Pipeline script from SCM**
* Add Git repo URL
* Script Path: `Jenkinsfile`

---

### Step 9: Run Pipeline

Click **Build Now**

Expected Output:

```
Building application...
Running tests...
Scanning code...
Deploying application...
```

---

### Step 10: Role-Based Access Control

* Install Role-based Authorization plugin
* Configure roles:

  * Admin
  * Developer

---

## 🔁 Pipeline Workflow

1. Developer pushes code to GitHub
2. Jenkins fetches repository
3. Shared library is loaded
4. Pipeline executes:

   * Build
   * Test
   * Scan
   * Deploy

---

## ✅ Key Features

* Centralized CI/CD system
* Reusable pipeline logic
* Multi-application support
* Consistent execution across projects

---

## 📸 Output

* Multiple pipelines running successfully
* Same pipeline stages for all applications
* Shared library enforced across projects

![image alt](https://github.com/Arjun-Nalge/Centralized-CI-CD-Platform-using-Jenkins-Shared-Library/blob/0cbe795d7c4ca58031c1d795b6bdcd16d839b52c/Screenshot%202026-04-23%20114108.png)
![image alt](https://github.com/Arjun-Nalge/Centralized-CI-CD-Platform-using-Jenkins-Shared-Library/blob/2c8184592fa04455f14b1b8447ac805d6113a578/Screenshot%202026-04-23%20114422.png)
![image alt](https://github.com/Arjun-Nalge/Centralized-CI-CD-Platform-using-Jenkins-Shared-Library/blob/9c61e76ba1e7e143565f8921759e628041ec2902/Screenshot%202026-04-23%20114402.png)

---

## 🚀 Conclusion

This project demonstrates how Jenkins shared libraries can be used to build a centralized CI/CD platform, improving consistency, scalability, and maintainability across multiple applications.

---

## 📂 Repositories

* Shared Library Repo (https://github.com/Arjun-Nalge/jenkins-shared-library.git)
* app-1 Repo (https://github.com/Arjun-Nalge/app-1.git)
* app-2 Repo (https://github.com/Arjun-Nalge/app-2.git)

---

## Author
Arjun Nalge - DevOps Engineer

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://www.linkedin.com/in/arjun-nalge-313642398)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?logo=github)](https://github.com/Arjun-Nalge/Arjun-Nalge.git)
