# DevOps Assessment: Yii2 Application Deployment with Docker Swarm and GitHub Actions

## 📦 Project Overview

This project demonstrates the deployment of a Yii2 PHP application using Docker Swarm, NGINX reverse proxy, and CI/CD via GitHub Actions. Ansible automates the setup of the EC2 instance, while GitHub Actions handles containerization and deployment.

---

## Setup Instructions

1. **Clone this repository**
```bash
git clone https://github.com/Vinayvasantham/DevOps-Assessment.git
cd DevOps-Assessment
```

2. Set up AWS EC2 instance
 * Launch an Ubuntu EC2 instance.

 > Allow ports: 22, 80, and 9000 (or 9001).

* SSH into the instance using your key pair:
```bash
ssh -i mykeypair.pem ubuntu@<EC2-PUBLIC-IP>
```

3. Run Ansible Playbook
On your local machine (WSL or control host):
```bash
cd ansible/
ansible-playbook -i inventory playbook.yml
```
This installs Docker, Docker Compose, NGINX, Git, PHP dependencies, initializes Docker Swarm, configures NGINX, clones the app, and deploys it as a Swarm stack.

CI/CD Pipeline (GitHub Actions)
Trigger
. Triggered on push to main branch.

Workflow
. Builds Docker image from /app/Dockerfile.

. Pushes image to Docker Hub.

. SSHes into the EC2 server.

.  Pulls new image.

. Updates Docker Swarm service.

Secrets Required
Set these in GitHub → Settings → Secrets and variables → Actions → New repository secret:

. HOST: Public IP or DNS of EC2 instance

. USERNAME: SSH username (ubuntu)

. PRIVATE_KEY: Private key contents (e.g., mykeypair.pem)

. DOCKERHUB_USERNAME: Docker Hub username

. DOCKERHUB_PASSWORD: Docker Hub password or token

Assumptions
. The Yii2 application code is placed in /app and contains a valid composer.json.

. Ports 80 (for NGINX) and 9001 (PHP-FPM) are available on the EC2 instance.

. Docker is installed and Swarm is initialized.

. Dockerfile and docker-compose.yml are present in the root or app/.

How to Test Deployment
. Push code changes to main branch:
```bash
git add .
git commit -m "Your message"
git push origin main
```
. Go to GitHub → Actions → Monitor workflow.

. Access the app in browser:
```bash
http://<EC2-PUBLIC-IP>/
```

🧹 Cleanup (Optional)
To remove the stack:
```bash
ssh -i mykeypair.pem ubuntu@<EC2-PUBLIC-IP>
docker stack rm yii2_app
```

📬 Contact
For any queries, reach out at vinayvasantham

---

Let me know if you want a custom version based on your exact ports, file paths, or workflow names.
