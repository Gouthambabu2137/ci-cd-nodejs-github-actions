# CI/CD Pipeline for Node.js Application

This repository demonstrates a complete CI/CD pipeline setup for a simple Node.js web application. The project uses GitHub Actions for automation and Docker for containerization.

---

## Project Overview

- **Application**: Node.js web server (sample app)
- **CI/CD Tool**: GitHub Actions
- **Container Platform**: Docker
- **Image Registry**: DockerHub
- **Trigger**: On push to the `main` branch

When changes are pushed to the `main` branch:
1. GitHub Actions checks out the code.
2. Docker Buildx is initialized.
3. DockerHub login is handled using GitHub Secrets.
4. The Docker image is built and pushed to DockerHub.

---

## Project Structure

ci-cd-nodejs-github-actions/
├── .github/
│ └── workflows/
│ └── main.yml # GitHub Actions workflow definition
├── Dockerfile # Docker instructions for building the app
├── app.js # Basic Node.js web app
├── package.json # Node.js project metadata and dependencies
├── .gitignore # Git ignored files
└── README.md # Project documentation 


---

## GitHub Actions Workflow (`main.yml`)

This workflow is stored in `.github/workflows/main.yml` and performs the following:

### Trigger
- Automatically runs on every push to the `main` branch.

### Jobs
- **Checkout Code**
- **Set Up Docker Buildx**
- **Log in to DockerHub**
- **Build and Push Docker Image**

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ "main" ]

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Docker
        uses: docker/setup-buildx-action@v2

      - name: Log in to DockerHub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and Push Docker image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ secrets.DOCKER_USERNAME }}/goutham-node-app:latest

Dockerfile:

FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "app.js"]

####GitHub Secrets Configuration####
The workflow uses GitHub Secrets to log in to DockerHub. Set these secrets in your repository:

DOCKER_USERNAME → Your DockerHub username

DOCKER_PASSWORD → Your DockerHub access token or password

Add these under:
GitHub Repo → Settings → Secrets and variables → Actions → Repository secrets

AUTHOR
GOUTHAM BABU
GitHub: https://github.com/Gouthambabu2137
