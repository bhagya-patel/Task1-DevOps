# Task1-DevOps
🚀 CI/CD Pipeline: Automate Code Deployment using GitHub Actions
📌 Objective
To set up a CI/CD pipeline using GitHub Actions that builds and deploys a sample Node.js web application using Docker.

🛠️ Tools Used
GitHub

GitHub Actions

Node.js

Docker

📁 Project Structure
bash
Copy
Edit
.
├── app/
│   └── index.js          # Sample Node.js app
├── Dockerfile            # Docker configuration
├── .github/
│   └── workflows/
│       └── ci-cd.yml     # GitHub Actions workflow
├── .dockerignore
└── README.md
🔄 CI/CD Workflow Overview
Trigger: On every push to the main branch

Build Stage:

Install Node.js dependencies

Run tests

Build Docker image

Deploy Stage:

Push Docker image to DockerHub (or GHCR)

Optionally deploy to cloud or container service

🧪 Sample .github/workflows/ci-cd.yml
yaml
Copy
Edit
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

      - name: Log in to DockerHub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build Docker image
        run: docker build -t ${{ secrets.DOCKER_USERNAME }}/ci-cd-app:latest .

      - name: Push Docker image
        run: docker push ${{ secrets.DOCKER_USERNAME }}/ci-cd-app:latest
💡 Interview Questions & Answers
2. How do GitHub Actions work?
GitHub Actions uses YAML workflows stored in .github/workflows/. When a specified event (like a push) occurs, GitHub triggers the defined workflow, executing jobs and steps using runners.

3. What are runners?
Runners are virtual machines that run your GitHub Actions workflows. GitHub provides hosted runners (Ubuntu, Windows, macOS), or you can use self-hosted runners for custom environments.

4. Difference between Jobs and Steps
Aspect	Jobs	Steps
Scope	Entire environment (VM)	Single task within a job
Execution	Can run in parallel	Run sequentially in a job
Container	Can define a container per job	Steps share the job’s container

5. How to secure secrets in GitHub Actions?
Use GitHub's Secrets feature under Repo Settings > Secrets and Variables.

Reference secrets securely like: ${{ secrets.MY_SECRET_KEY }}

Secrets are encrypted at rest and in transit.

6. How to handle deployment errors?
Add continue-on-error: false (default) to stop on failure

Use try-catch style scripting or if: conditions

Enable Slack/email notifications on failure

Log error messages using run: echo "Error..."

7. Explain the Docker build-push workflow.
bash
Copy
Edit
docker login             # Authenticate to DockerHub
docker build -t image .  # Build the image from Dockerfile
docker push image        # Push the image to registry
These steps are often automated in CI/CD to ensure consistent image delivery.

8. How can you test a CI/CD pipeline locally?
Use tools like act to simulate GitHub Actions locally

bash
Copy
Edit
act push
Use docker-compose or minikube to test deployments

Run scripts manually and validate output before pushing

✅ Final Deliverables
