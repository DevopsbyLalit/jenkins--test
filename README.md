# Jenkins GitHub Webhook Deployment

## Project Overview

This project demonstrates a simple CI/CD pipeline using:

- GitHub
- Jenkins
- Apache HTTP Server
- Amazon EC2
- HTML Website

Whenever code is pushed to GitHub, Jenkins automatically pulls the latest code and deploys it to the Apache web server.

---

## Architecture

```
VS Code
   │
   ▼
Git
   │
   ▼
GitHub Repository
   │
   ▼
GitHub Webhook
   │
   ▼
Jenkins
   │
   ▼
Apache Web Server
   │
   ▼
Live Website
```

---

## Technologies Used

- AWS EC2 (Amazon Linux 2023)
- Jenkins
- Apache HTTP Server
- Git
- GitHub
- HTML
- Linux

---

## Prerequisites

- Java 21
- Jenkins Installed
- Apache Installed
- Git Installed
- GitHub Repository

---

## Install Apache

```bash
sudo dnf install httpd -y
sudo systemctl enable httpd
sudo systemctl start httpd
```

---

## Install Git

```bash
sudo dnf install git -y
```

---

## Jenkins Build Step

```bash
rm -rf /var/www/html/*
cp index.html /var/www/html/
```

---

## Deployment Process

1. Create HTML project in VS Code
2. Push project to GitHub
3. Jenkins clones repository
4. Jenkins copies website files to Apache Document Root
5. Apache serves updated website

---

## Git Commands

Initialize Repository

```bash
git init
```

Add Files

```bash
git add .
```

Commit

```bash
git commit -m "Initial Commit"
```

Add Remote

```bash
git remote add origin https://github.com/username/repository.git
```

Push

```bash
git push -u origin main
```

---

## Jenkins Configuration

Source Code Management

Git Repository URL

```
https://github.com/username/repository.git
```

Branch

```
*/main
```

Build Step

```
Execute Shell
```

Commands

```bash
rm -rf /var/www/html/*
cp index.html /var/www/html/
```

---

## Common Error

### Error

```
sudo: a password is required
```

### Reason

The Jenkins job runs as the **jenkins** user.

The deployment script used:

```bash
sudo cp index.html /var/www/html/
```

Since the Jenkins user was not allowed to execute sudo without a password, the build failed.

---

## Solution

Instead of using sudo inside the Jenkins build step, change the ownership of Apache's document root.

```bash
sudo chown -R jenkins:jenkins /var/www/html
sudo chmod -R 775 /var/www/html
```

After changing permissions, remove sudo from the deployment commands.

```bash
rm -rf /var/www/html/*
cp index.html /var/www/html/
```

Build runs successfully.

---

## Why This Solution?

Jenkins executes build jobs using the **jenkins** user.

Giving the Jenkins user ownership of the deployment directory removes the need for sudo and allows automated deployment.

---

## Future Improvements

- GitHub Webhooks
- Jenkins Pipeline
- Jenkinsfile
- Docker Deployment
- Nginx Deployment
- AWS CodeDeploy
- Kubernetes Deployment

---

## Author

Lalit

GitHub

https://github.com/DevopsbyLalit