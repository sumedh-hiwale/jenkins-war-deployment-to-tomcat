# 🚀 Jenkins WAR Deployment to Tomcat Using Jenkins Agent

## 📌 Project Overview

This project demonstrates how to build and deploy a Java WAR application on Apache Tomcat using Jenkins Master-Agent Architecture. Jenkins Server triggers the build, Maven generates the WAR file on Jenkins Agent, and the Deploy to Container Plugin deploys the application to Tomcat automatically.

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
| Jenkins Server | AWS EC2                    |
| Jenkins Agent  | AWS EC2                    |
| OS             | Ubuntu 24.04               |
| Java           | OpenJDK 21                 |
| Maven          | 3.8.7                      |
| Tomcat         | 10                         |
| Jenkins Plugin | Deploy to Container Plugin |

---

## 🍴 Repository Information

### Original Repository

```text
https://github.com/kunalc881/demo-java-war.git
```

### Forked Repository Used

```text
https://github.com/sumedh-hiwale/demo-java-war.git
```

### Issue Identified

The original repository used an older Maven WAR Plugin version which was incompatible with Java 21.

### Fix Applied

File Modified:

```text
pom.xml
```

Updated:

```xml
<artifactId>maven-war-plugin</artifactId>
<version>3.0.0</version>
```

To:

```xml
<artifactId>maven-war-plugin</artifactId>
<version>3.4.0</version>
```

Changes were committed to the forked repository and used for deployment.

---

# 🚀 Deployment Flow

### 1️⃣ Verify Jenkins Agent Connection

* Jenkins Agent connected successfully to Jenkins Server.

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

Credentials:

```text
Username : tomcat
Password : 1
```

---

### 6️⃣ Install Deploy to Container Plugin

```text
Manage Jenkins
→ Plugins
→ Available Plugins
→ Deploy to Container Plugin
```

---

### 7️⃣ Create Tomcat Credentials

```text
Manage Jenkins
→ Credentials
→ System
→ Global
→ Add Credentials
```

Type:

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

### 8️⃣ Create Freestyle Job

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

### 9️⃣ Configure Git Repository

Repository URL:

```text
https://github.com/sumedh-hiwale/demo-java-war.git
```

Branch:

```text
*/master
```

---

### 🔟 Configure Maven Build

Build Step:

```text
Invoke top-level Maven targets
```

Goals:

```text
clean install
```

---

### 1️⃣1️⃣ Configure Deployment

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

### 1️⃣2️⃣ Build and Deploy

Trigger Build:

```text
Build Now
```

Successful Output:

```text
DeployPublisher][INFO] Attempting to deploy 1 war file(s)

DeployPublisher][INFO] Deploying demo.war to container Tomcat 9.x Remote with context sumedh

Finished: SUCCESS
```

---

## 🌐 Application Verification

Application URL:

```text
http://54.221.97.230:8080/sumedh
```

Verification:

✅ Application opened successfully

✅ WAR deployed successfully

✅ Jenkins to Tomcat deployment completed

---

## 📂 Files Modified

### Jenkins Agent

```text
/etc/tomcat10/tomcat-users.xml
```

### GitHub Repository

```text
pom.xml
```

---

## 🎯 Learning Outcomes

* Jenkins Master-Agent Architecture
* Maven Build Automation
* Apache Tomcat Administration
* Jenkins Credentials Management
* Deploy to Container Plugin
* WAR Deployment Automation
* Build Troubleshooting
* Repository Forking and Code Fixes
* End-to-End CI/CD Deployment Workflow

---

## ✅ Result

✔ Jenkins Server Connected to Agent

✔ Maven Installed Successfully

✔ Tomcat Installed Successfully

✔ Tomcat Manager Configured

✔ Jenkins Credentials Created

✔ WAR File Generated Successfully

✔ WAR File Deployed Successfully

✔ Application Accessible via Browser

✔ End-to-End Jenkins → Maven → Tomcat Deployment Completed Successfully 🎉

---

## 📖 Flow Summary

```text
Jenkins Server
      ↓
Trigger Build
      ↓
Jenkins Agent
      ↓
Git Clone (Forked Repository)
      ↓
Maven Build
      ↓
WAR File Generated
      ↓
Deploy to Container Plugin
      ↓
Tomcat Server
      ↓
Application Deployment
      ↓
Browser Verification
```

🎉 Demo Completed Successfully
