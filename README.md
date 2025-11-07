# Jenkins + Docker CI/CD Demo

## 🚀 Project Overview
This is a simple Node.js web app demonstrating a full CI/CD pipeline using Jenkins and Docker.

## 🏗️ Pipeline Steps
1. Clone code from GitHub
2. Install dependencies
3. Run tests
4. Build Docker image
5. Run container automatically

## 🧰 Tech Stack
- Jenkins (running in Docker)
- Node.js
- Docker Desktop (Windows)
- GitHub

## ⚙️ Run Locally

```bash
docker build -t jenkins-demo-app .
docker run -d -p 3000:3000 jenkins-demo-app
