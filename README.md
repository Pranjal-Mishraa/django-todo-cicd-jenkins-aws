# CI/CD Pipeline for Django TODO App (Jenkins + Docker)

This project implements a CI/CD pipeline using Jenkins and Docker for a Django-based TODO web application running on port 8000. The pipeline automates code checkout, dependency installation, testing, Docker image build, container deployment, and basic verification.

## Pipeline Overview

The Jenkins pipeline (`Jenkinsfile`) is defined using Declarative Pipeline syntax and runs on any available Jenkins agent:

- Checks out the latest code from the connected Git repository.
- Creates a Python virtual environment and installs dependencies from `requirements.txt`.
- Runs Django test cases to validate the application.
- Builds a Docker image for the application.
- Deploys the application as a Docker container.
- Verifies deployment by listing running containers and available images .

## Jenkinsfile Stages

The pipeline is composed of the following stages:

1. **Checkout**
   - Uses `checkout scm` to pull the latest source code from the configured Git repository.

2. **Create Virtual Environment & Install Dependencies**
   - Creates a Python virtual environment using `python3 -m venv venv`.
   - Activates the virtual environment.
   - Upgrades `pip` and installs all dependencies from `requirements.txt`.

3. **Run Django Tests**
   - Activates the virtual environment.
   - Changes directory to the application folder (`APP_DIR`, e.g. `app`).
   - Executes `python manage.py test` to run Django tests and ensure the build is stable [web:26][web:29].

4. **Build Docker Image**
   - Builds a Docker image with the tag `${IMAGE_NAME}:latest` using the project’s `Dockerfile`.
   - Default `IMAGE_NAME` is `django-todo-app`.

5. **Deploy Docker Container**
   - Stops any existing container with the same name (`${CONTAINER_NAME}`) if running.
   - Removes the stopped container to avoid name conflicts.
   - Runs a new container in detached mode (`-d`), mapping `${APP_PORT}` (default `8000`) from the container to the host:
     - `docker run -d --name ${CONTAINER_NAME} -p ${APP_PORT}:${APP_PORT} ${IMAGE_NAME}:latest` .

6. **Verify Deployment**
   - Runs `docker ps` to show active containers.
   - Runs `docker images` to list available images, confirming that the new deployment is live.

## Environment Configuration

The pipeline uses an `environment` block to keep important values configurable:

```groovy
environment {
    APP_DIR = "app"
    IMAGE_NAME = "django-todo-app"
    CONTAINER_NAME = "django-todo-app"
    APP_PORT = "8000"
}
```

You can change these values to match your folder structure, image naming convention, and port configuration without modifying the rest of the pipeline script.

## Prerequisites

To run this pipeline successfully, you need:

- A Jenkins server with:
  - Pipeline plugin installed.
  - Docker installed and accessible to Jenkins.
  - Python 3 installed on the Jenkins agent.
- Docker installed on the machine where the pipeline runs.
- Git repository containing:
  - Django TODO application code inside the `app/` directory (or your chosen `APP_DIR`).
  - `requirements.txt` file.
  - `Dockerfile` configured to expose port 8000 and run the Django app .

## How to Use

1. **Clone the repo locally (optional):**

```bash
git clone https://github.com/your-username/django-todo-cicd.git
cd django-todo-cicd
```

2. **Configure Jenkins:**
- Create a new Pipeline job.
- Point the job’s SCM configuration to this Git repository.
- In the Pipeline section, choose “Pipeline script from SCM” or paste the `Jenkinsfile` directly [web:24][web:25].

3. **Run the pipeline:**
- Click **Build Now** in Jenkins.
- Watch the console output for each stage (Checkout, venv setup, tests, Docker build, Deploy, Verify).
- After a successful run, access the application at:

```bash
http://<server-ip>:8000
```

(replace `<server-ip>` with your Jenkins/host machine IP).

## What this project demonstrates

- A practical CI/CD pipeline using Jenkins for a Django web application.
- Automated creation of isolated Python environments per build.
- Integration of automated tests into the pipeline to prevent bad deployments.
- Containerization of the application with Docker.
- Automated deployment and basic verification using Docker commands .

## Future Enhancements

- Push the Docker image to Docker Hub or a private registry.
- Add stages for code quality tools (e.g., SonarQube).
- Deploy the container to a cloud VM (AWS EC2) instead of local Docker.
- Add monitoring/alerts for container health .

## Author

Pranjal Mishra – DevOps Engineer  
LinkedIn: https://www.linkedin.com/in/-pranjal-mishra/
