# 🚀 Jenkins WAR Deployment to Tomcat using Jenkins Agent

## 📌 Project Overview

This project demonstrates how to build a Java WAR application using Jenkins and deploy it to an Apache Tomcat server running on a Jenkins Agent (Linux Slave).

---

## 🏗️ Architecture

```text
Jenkins Server (Master)
        │
        │ SSH Connection
        ▼
Jenkins Agent (Linux-Slave)
        │
        ├── Maven Build
        ├── WAR Generation
        └── Tomcat Deployment
```

---

## 🖥️ Environment Details

| Component      | Details                    |
| -------------- | -------------------------- |
| Jenkins Server | EC2 Instance               |
| Jenkins Agent  | EC2 Instance               |
| OS             | Ubuntu 24.04               |
| Java           | OpenJDK 21                 |
| Maven          | 3.8.7                      |
| Tomcat         | 10                         |
| Jenkins Plugin | Deploy to Container Plugin |

---

## 🔄 Deployment Flow

### 1️⃣ Verify Jenkins Agent Connection

✔ Jenkins Agent connected successfully to Jenkins Server.

---

### 2️⃣ Install Maven on Jenkins Agent

```bash
sudo apt update
sudo apt install maven -y
```

Verify:

```bash
mvn -version
```

---

### 3️⃣ Install Tomcat on Jenkins Agent

```bash
sudo apt update
sudo apt install tomcat10 tomcat10-admin -y
```

Start Tomcat:

```bash
sudo systemctl enable tomcat10
sudo systemctl restart tomcat10
```

Verify:

```bash
sudo ss -lntp | grep 8080
```

---

### 4️⃣ Configure Tomcat Users

Edit:

```bash
sudo vi /etc/tomcat10/tomcat-users.xml
```

Add:

```xml
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="admin-gui"/>

<user username="tomcat"
      password="1"
      roles="manager-gui,manager-script,admin-gui"/>
```

Restart Tomcat:

```bash
sudo systemctl restart tomcat10
```

---

### 5️⃣ Verify Tomcat Manager Access

Manager URL:

```text
http://<Agent-IP>:8080/manager/html
```

Host Manager URL:

```text
http://<Agent-IP>:8080/host-manager/html
```

Login Credentials:

```text
Username: tomcat
Password: 1
```

---

### 6️⃣ Install Jenkins Deployment Plugin

```text
Manage Jenkins
→ Plugins
→ Available Plugins
→ Deploy to Container Plugin
```

Install Plugin Successfully.

---

### 7️⃣ Create Tomcat Credentials in Jenkins

```text
Manage Jenkins
→ Credentials
→ System
→ Global
→ Add Credentials
```

Credential Type:

```text
Username with Password
```

Values:

```text
Username : tomcat
Password : 1
ID       : tomcat-creds
```

---

### 8️⃣ Fork Demo Repository

Original Repository:

```text
https://github.com/kunalc881/demo-java-war.git
```

Fork Repository to your GitHub account.

---

### 9️⃣ Fix Maven WAR Plugin Issue

Edit:

```text
pom.xml
```

Replace:

```xml
<version>3.0.0</version>
```

With:

```xml
<version>3.4.0</version>
```

Commit Changes.

---

### 🔟 Create Jenkins Freestyle Job

Job Name:

```text
deploy-war-to-tomcat
```

Enable:

```text
Restrict where this project can be run
```

Label:

```text
Linux-Slave
```

---

### 1️⃣1️⃣ Configure Git Repository

Repository URL:

```text
https://github.com/<your-github-username>/demo-java-war.git
```

Branch:

```text
*/master
```

---

### 1️⃣2️⃣ Configure Maven Build

Build Step:

```text
Invoke top-level Maven targets
```

Goals:

```text
clean install
```

---

### 1️⃣3️⃣ Configure Tomcat Deployment

Post Build Action:

```text
Deploy war/ear to a container
```

WAR File:

```text
**/*.war
```

Context Path:

```text
sumedh
```

Container:

```text
Tomcat 9.x Remote
```

Credentials:

```text
tomcat-creds
```

Tomcat URL:

```text
http://54.221.97.230:8080
```

---

### 1️⃣4️⃣ Build and Deploy

Trigger Build:

```text
Build Now
```

Successful Output:

```text
Deploying demo.war
Finished: SUCCESS
```

---

## 🌐 Application Verification

Application URL:

```text
http://54.221.97.230:8080/sumedh
```

Output:

```text
Welcome to Jenkins
Welcome to Chhatrapati Sambhajinagar
```

---

## ✅ Result

✔ Jenkins Agent Connected Successfully

✔ Maven Installed Successfully

✔ Tomcat Installed Successfully

✔ WAR File Generated Successfully

✔ WAR File Deployed Successfully

✔ Application Accessible via Browser

✔ End-to-End Jenkins to Tomcat Deployment Completed Successfully 🎉

---

## 🎯 Learning Outcomes

* Jenkins Master-Agent Architecture
* Maven Build Automation
* Apache Tomcat Configuration
* Jenkins Credentials Management
* Deploy to Container Plugin
* WAR Deployment Automation
* End-to-End CI/CD Deployment Workflow
