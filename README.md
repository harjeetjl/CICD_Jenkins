
# Jenkins CI/CD Pipeline for Python Flask Application

## Overview

This guide outlines the steps to set up a Jenkins CI/CD pipeline for a Python Flask web application. The pipeline performs the following:

- Clone the repository from GitHub
- Install project dependencies
- Run unit tests using `pytest`
- Deploy the application to an EC2 instance
- Send email notifications on build success or failure

## Repository

Clone or fork the following repository to begin:  
[Sample Flask Application](https://github.com/harjeetjl/CICD_Jenkins.git)

## Prerequisites

- Python 3.x and pip
- Jenkins installed and running
- Required Jenkins plugins:
  - Git
  - Pipeline
  - Email Extension
  - SSH Agent Plugin
- GitHub repository with Flask app and `Jenkinsfile`

---

## Jenkins Setup

### 1. Jenkins Installation

Install Jenkins locally or on a VM (e.g., AWS EC2). You may also use a managed Jenkins service.

Refer to the [Jenkins Installation Guide](https://www.jenkins.io/doc/book/installing/).

### 2. Install Required Tools on Jenkins Server

```bash
sudo yum update
sudo yum install python3 python3-pip
pip3 install virtualenv pytest
```

### 3. Install Jenkins Plugins

From Jenkins Dashboard:

- Go to Manage Jenkins > Plugin Manager
- Install:
  - Git
  - Pipeline
  - Email Extension
  - SSH Agent

### 4. Add MongoDB URI as Jenkins Credential

- Go to Jenkins Dashboard > Manage Jenkins > Credentials
- Choose the global domain
- Add a new credential:
  - Kind: Secret text
  - Secret: Your MongoDB URI
  - ID: `harjeet-MONGO-URI`
  - Description: MongoDB connection string

### 5. Add EC2 SSH Private Key in Jenkins

- Go to Jenkins Dashboard > Manage Jenkins > Credentials
- Add new credentials:
  - Kind: SSH Username with private key
  - Username: `ec2-user`
  - Private Key: Paste your `.pem` content
  - ID: `harjeet-ec2-ssh-key`

---

## Pipeline Stages

### Clone Repo

The pipeline checks out the `main` branch from the GitHub repository.

### Install Dependencies

- Creates a Python virtual environment
- Installs required packages using `pip`

### Run Tests

- Executes `pytest` using the MongoDB URI as an environment variable

### Deploy to EC2

- Connects to a remote EC2 instance via SSH
- Installs Git, Python3, and pip if not already installed
- Clones or pulls the latest repository changes
- Updates the `.env` file with MongoDB URI and port configuration

---

## Webhook Trigger

### GitHub Webhook Configuration

- Go to GitHub Repository Settings > Webhooks
- Add a webhook:
  - Payload URL: `http://<JENKINS_URL>/github-webhook/`
  - Content Type: `application/json`
  - Trigger: Push events

### Jenkins Project Configuration

- Enable "GitHub hook trigger for GITScm polling" under Build Triggers

---

## Email Notifications

Email alerts are sent based on build status:

- **On Success**: Notifies deployment success
- **On Failure**: Notifies build failure

Configure email under Jenkins > Manage Jenkins > Configure System.

---

## Screnshots
1. Jenkins Success
![Screenshot 2025-05-31 183522](https://github.com/user-attachments/assets/c4a19ac6-9721-4b67-a562-7aa941e5a58e)

2. Pipeline Log
![Screenshot 2025-05-31 183701](https://github.com/user-attachments/assets/21c00e17-76bb-4ba2-ae64-0031e3cc8344)

3. Stages Overview
![Screenshot 2025-05-31 183857](https://github.com/user-attachments/assets/f9e916bf-e31a-42ff-a57f-3ed44fe25d40)

4. Mail Triggers (Success and Failure)
![Screenshot 2025-05-31 184611](https://github.com/user-attachments/assets/2aeb2602-985c-4d57-83d4-fe893b59ed10)
![Screenshot 2025-05-31 184505](https://github.com/user-attachments/assets/db0fa60e-c179-4c35-aded-2c2c65bf0dff)

---
