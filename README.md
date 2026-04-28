# Docker + Jenkins + AWS ECR Project

## 📌 Overview
This project demonstrates CI/CD using:
- Docker
- Jenkins
- AWS ECR

## 🚀 Workflow
1. Code pushed to GitHub
2. Jenkins builds Docker image
3. Image pushed to AWS ECR

## 🐳 Run Locally
docker build -t myapp .
docker run -p 5000:5000 myapp
