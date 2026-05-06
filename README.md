# DevOps WordPress Deployment Project

## Project Overview

This project demonstrates a basic DevOps workflow using:

* Terraform for infrastructure provisioning
* Docker Compose for containerized deployment
* WordPress + MySQL application stack
* Nginx reverse proxy
* GitHub for version control
* Jenkins for CI/CD pipeline setup
* AWS EC2 Ubuntu server hosting

The goal of the project was to understand infrastructure provisioning, container orchestration, CI/CD basics, and cloud deployment.

---

# Technologies Used

| Technology     | Purpose                       |
| -------------- | ----------------------------- |
| Terraform      | Infrastructure provisioning   |
| AWS EC2        | Cloud virtual machine         |
| Ubuntu 22.04   | Server operating system       |
| Docker         | Containerization              |
| Docker Compose | Multi-container orchestration |
| WordPress      | Web application               |
| MySQL          | Database                      |
| Nginx          | Reverse proxy                 |
| Jenkins        | CI/CD automation              |
| GitHub         | Source code management        |

---

# Project Architecture

```text
GitHub Repository
        ↓
Jenkins Pipeline
        ↓
SSH into EC2 Server
        ↓
Docker Compose Deployment
        ↓
Nginx Reverse Proxy
        ↓
WordPress + MySQL Containers
```

---

# AWS Infrastructure

## EC2 Instance

* Ubuntu Server
* Hosted on AWS EC2
* Security groups configured for:

  * SSH (22)
  * HTTP (80)
  * Jenkins (8080)

---

# Dockerized Application Stack

The application stack contains:

## Services

### 1. WordPress

* Runs inside Docker container
* Connected to MySQL database
* Accessible through Nginx reverse proxy

### 2. MySQL

* Stores WordPress data
* Configured using environment variables

### 3. Nginx

* Reverse proxy server
* Routes traffic to WordPress container

---

# Docker Compose Configuration

The `docker-compose.yml` file manages:

* WordPress container
* MySQL container
* Nginx container
* Volumes and networking

Deployment command:

```bash
docker compose up -d
```

---

# Nginx Reverse Proxy

Nginx was configured to:

* Listen on port 80
* Forward requests to WordPress container
* Handle HTTP traffic routing

Example configuration:

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://wordpress:80;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

---

# GitHub Repository

Project files were stored and managed using GitHub.

## Repository Contents

* docker-compose.yml
* nginx.conf
* Jenkinsfile
* prometheus.yml
* README.md
* .gitignore

---

# Jenkins CI/CD Setup

Jenkins was installed on the EC2 server.

## Pipeline Goals

The Jenkins pipeline was configured to:

1. Pull source code from GitHub
2. Execute deployment steps
3. SSH into EC2
4. Run Docker Compose deployment commands

---

# Jenkinsfile

Example pipeline:

```groovy
pipeline {
    agent any

    stages {

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@SERVER-IP << EOF
                    cd ~/devops-wordpress-project
                    docker compose down
                    docker compose up -d
                    EOF
                    '''
                }
            }
        }

    }
}
```

---

# Terraform Usage

Terraform was used to provision AWS infrastructure.

## Provisioned Resources

* EC2 instance
* Security group
* Public IP access

Terraform commands used:

```bash
terraform init
terraform plan
terraform apply
```

---

# Security Group Rules

| Port | Purpose        |
| ---- | -------------- |
| 22   | SSH Access     |
| 80   | HTTP Access    |
| 8080 | Jenkins Access |

---

# Problems Faced During Project

## 1. WordPress Redirect Loop

Issue:

* ERR_TOO_MANY_REDIRECTS

Fix:

* Corrected WordPress URL configuration
* Fixed reverse proxy configuration

---

## 2. Jenkins Plugin Compatibility

Issue:

* Pipeline plugins required newer Jenkins version

Fix:

* Installed Jenkins WAR manually
* Updated Java version to Java 21

---

## 3. SSH Connection Issues

Issue:

* Permission denied
* SSH timeout problems

Fix:

* Security group corrections
* Proper SSH key configuration

---

## 4. Docker Compose Command Issues

Issue:

* `docker-compose` not found

Fix:

* Used modern Docker Compose syntax:

```bash
docker compose up -d
```

---

# Monitoring

Prometheus configuration file (`prometheus.yml`) was added for future monitoring setup.

---

# Learning Outcomes

This project helped in understanding:

* Cloud infrastructure provisioning
* Containerized deployments
* Reverse proxy configuration
* CI/CD pipeline concepts
* GitHub integration
* Jenkins automation basics
* Troubleshooting real deployment issues

---

# Future Improvements

Possible future enhancements:

* Add HTTPS using Nginx + SSL
* Add Grafana monitoring dashboard
* Configure automated GitHub webhooks
* Use Docker volumes for persistent storage
* Add backup automation
* Improve security hardening

---

# Conclusion

This project demonstrates a complete beginner-level DevOps workflow integrating cloud infrastructure, containerization, CI/CD automation, and version control.

The project provided practical experience with deployment debugging, infrastructure management, and automation concepts using real cloud resources.

