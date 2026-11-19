# CI/CD Deployment Pipeline with Automated Monitoring

A CI/CD pipeline that automates the deployment of a Flask application to an AWS EC2 instance using GitHub Actions and Docker, with AWS CloudWatch monitoring for EC2 CPU utilization.

## Overview

This project automates the deployment process of a Flask application, reducing manual deployment steps to a single `git push` to the `main` branch.

Whenever code is pushed to the main branch, GitHub Actions automatically connects to the AWS EC2 instance through SSH, retrieves the latest application code, builds a Docker image, and deploys the updated application.

AWS CloudWatch is configured with an alarm to monitor the EC2 instance's CPU utilization.

## Technologies Used

- **Python (Flask):** Used to develop the web application.
- **Docker:** Used to containerize the Flask application and its dependencies.
- **Git & GitHub:** Used for version control and source code management.
- **GitHub Actions:** Automates the CI/CD deployment workflow.
- **AWS EC2:** Hosts the Dockerized Flask application.
- **AWS CloudWatch:** Monitors EC2 CPU utilization through a configured alarm.
- **SSH:** Enables secure communication between GitHub Actions and the EC2 instance.

## Architecture

The deployment workflow follows this sequence:

```text
Developer
    |
    | Git Push to Main Branch
    v
GitHub Repository
    |
    v
GitHub Actions
    |
    | SSH Connection
    v
AWS EC2 Instance
    |
    | Git Pull
    v
Latest Application Code
    |
    | Docker Build
    v
Docker Image
    |
    | Run Container
    v
Flask Application
    |
    v
Live Application

AWS CloudWatch
    |
    v
Monitors EC2 CPU Utilization
    |
    v
CloudWatch Alarm
```

## Project Structure

```text
devops-mini-project/
│
├── .github/
│   └── workflows/
│       └── docker-build.yml
│
├── app.py
├── Dockerfile
├── requirements.txt
└── README.md
```

## Application Development

The application is developed using Flask and provides two endpoints:

- `/` – Returns the application status and a welcome message.
- `/health` – Returns the health status of the application.

The Flask application runs on port `5000` and is configured to accept connections from outside the container.

## Docker Configuration

A Dockerfile is used to package the Flask application and its dependencies into a Docker image.

The configuration:
- Uses Python 3.11 slim as the base image.
- Sets the working directory.
- Installs dependencies from `requirements.txt`.
- Copies the application files.
- Exposes port `5000`.
- Starts the Flask application.

## CI/CD Pipeline Using GitHub Actions

The GitHub Actions workflow is configured to trigger automatically whenever code is pushed to the `main` branch.

### Deployment Process

1. **Trigger:** A push to the main branch initiates the GitHub Actions workflow.
2. **SSH Connection:** GitHub Actions connects securely to the AWS EC2 instance using SSH.
3. **Code Retrieval:** The latest application code is cloned or pulled from the GitHub repository.
4. **Docker Build:** Docker builds a new image using the updated application code.
5. **Container Replacement:** The existing application container is stopped and removed.
6. **Deployment:** A new Docker container is started with the updated application.

The application is accessible through port `5001` on the EC2 instance, which maps to port `5000` inside the Docker container.

## AWS EC2 Deployment

An AWS EC2 instance is used as the deployment environment.

The EC2 instance:
- Hosts the Dockerized Flask application.
- Runs the GitHub Actions deployment commands through SSH.
- Retrieves the latest source code from GitHub.
- Builds and runs the Docker container.

The application is exposed on port `5001`.

## AWS CloudWatch Monitoring

AWS CloudWatch is integrated to monitor the CPU utilization of the EC2 instance.

A CloudWatch alarm is configured to monitor the EC2 CPU utilization metric.

This helps identify high CPU utilization on the instance and provides visibility into its resource usage.

## GitHub Secrets Configuration

The following GitHub repository secrets are configured to establish a secure SSH connection to the EC2 instance:

| Secret | Description |
|---|---|
| `EC2_HOST` | Public IP address or hostname of the EC2 instance |
| `EC2_USER` | SSH username of the EC2 instance |
| `EC2_SSH_KEY` | Private SSH key used for authentication |

These secrets prevent sensitive connection details from being hardcoded in the workflow file.

## How to Run Locally

### Prerequisites

- Python 3.11 or later
- Docker
- Git

### Steps

**1. Clone the repository**

```bash
git clone https://github.com/niv7778/devops-mini-project.git
cd devops-mini-project
```

**2. Build the Docker image**

```bash
docker build -t devops-mini-project .
```

**3. Run the Docker container**

```bash
docker run -d --name app -p 5001:5000 devops-mini-project
```

**4. Access the application**

Open the following URLs in your browser:

```text
http://localhost:5001/
http://localhost:5001/health
```

## Deployment Workflow

Once the project is configured with the required GitHub secrets and the EC2 instance is ready:

1. Make changes to the application.
2. Commit and push the changes to the `main` branch.
3. GitHub Actions automatically triggers the deployment workflow.
4. The updated application is deployed to the EC2 instance.
5. AWS CloudWatch continues monitoring EC2 CPU utilization through the configured alarm.

## Key Features

- Automated deployment triggered by GitHub pushes.
- Docker-based application containerization.
- Secure SSH-based deployment to AWS EC2.
- Automatic replacement of the previous application container.
- AWS CloudWatch alarm for EC2 CPU utilization monitoring.
- Reduced manual deployment effort through CI/CD automation.

## Project Outcome

Successfully implemented an automated CI/CD deployment pipeline for a Flask application using GitHub Actions, Docker, and AWS EC2, along with AWS CloudWatch monitoring for EC2 CPU utilization.

The project demonstrates automated application deployment, containerization, cloud hosting, and infrastructure monitoring.
