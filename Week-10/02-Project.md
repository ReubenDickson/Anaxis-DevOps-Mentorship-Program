WEEK 10 REAL-WORLD PROJECT
Manual Deployment Across Environments

Scenario: You manage a simple web application deployed via Docker.

Your company wants:

i. a dev environment

ii. a staging environment

iii. a production environment

Each must:

i. run the same application

ii. behave differently via configuration

iii. be deployable independently

Project Requirements
1. Application Setup

Create a simple web app (HTML or Node-based).

2. Docker Image (Single Image)

You must use one Docker image for all environments.

3. Environment-Specific Configuration

Run three containers:
``` bash
docker run -d -p 8081:80 -e APP_ENV=dev my-app
docker run -d -p 8082:80 -e APP_ENV=staging my-app
docker run -d -p 8083:80 -e APP_ENV=production my-app
```
Each environment must display:

i. its environment name

ii. current timestamp



4. Simulated Deployment

Update the application content.

Deploy to dev first, then staging, then production.

Document:

- order of deployment

- observed downtime

- risks noticed


What You Must Reflect On

You must answer:

i. What happens if prod is updated first?

ii. What breaks if config is hardcoded?

iii. How do you rollback manually?

iv. What risks did you observe?

Reflection is mandatory.

📁 STUDENT DELIVERABLES
``` bash
week10-deployment-project/
├── Dockerfile
├── app/
│   └── index.html
├── documentation/
│   ├── environment_design.md
│   ├── deployment_steps.md
│   ├── risks_observed.md
│   └── rollback_thoughts.md
└── README.md
```