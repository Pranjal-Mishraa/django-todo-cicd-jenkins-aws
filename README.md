# CI/CD Pipeline with Jenkins and AWS EC2

This project automates build and deployment using GitHub, Jenkins, Docker, and AWS EC2.

## Tech stack
- GitHub for source control.
- Jenkins for CI/CD automation.
- Docker for packaging the application.
- AWS EC2 for hosting the Jenkins server and running the application.

## Pipeline flow
1. Code is pushed to GitHub.
2. Jenkins pulls the latest changes.
3. Jenkins builds and deploys the application on EC2.
4. The application is accessed through the EC2 public IP.
