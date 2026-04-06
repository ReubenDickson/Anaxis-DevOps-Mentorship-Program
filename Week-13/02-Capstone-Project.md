CAPSTONE PROJECT — FULL SPECIFICATION

1. Application Setup

Students may reuse their existing Dockerized app.

Requirements:

- One application image
- Environment-based behavior using variables
- Logs enabled

``` bash
___
```
2. Environment Design

Create three environments:
``` bash
Environment         Port            Purpose
dev                 8081            Development
staging             8082            Pre-production
production          8083            Users
```
Each environment must:

- run the same image
- behave differently via config
- clearly identify itself in responses

``` bash
___
```

3. Deployment Simulation

Activities:

i. Deploy an update to dev

ii. Validate behavior

iii. Promote to staging

iv. Finally deploy to production

You must document:

- deployment order
- downtime observed
- risks encountered

``` bash
___
```

4. Scaling Attempt

For production:

i. Run at least 3 instances

ii. Use different ports

iii. Attempt to distribute traffic manually

Simulate:

- increased load
- user confusion
- operational overhead

``` bash
___
```

5. Failure Simulation

Activities:

i. stop one running instance

ii. observe impact

iii. attempt recovery manually

Document:

- response time
- affected users
- reliability gaps

``` bash
___
```

6. Rollback Simulation

Students must:

i. intentionally break something

ii. roll back to a previous version

iii. explain how rollback was achieved

``` bash
___
```

7. Reflection & Engineering Judgment

This section is mandatory and heavily weighted.

Answer the following questions:

i. What was hardest to manage?

ii. What caused the most risk?

iii. Where did downtime occur?

iv. What required constant attention?

v. What would fail at 10× scale?

vi. What should be automated next?

vii. Why is orchestration needed?

Honest answers matter more than perfection.
``` bash
📁 CAPSTONE SUBMISSION STRUCTURE
phase3-capstone/
├── app/
│   └── index.html
├── Dockerfile
├── documentation/
│   ├── environment_design.md
│   ├── deployment_journey.md
│   ├── scaling_attempt.md
│   ├── failure_simulation.md
│   ├── rollback_process.md
│   ├── operational_pain.md
│   └── why_orchestration.md
├── screenshots/
│   ├── running-containers.png
│   ├── environment-outputs.png
│   └── failure-test.png
└── README.md
```

Implenting the Capstone Project

Project Goal: To experience the manual lifecycle of a containerized application—from code to production—and understand the operational overhead of managing multiple environments without orchestration tools.

Phase 1: Iitialize the Project

``` bash
npx create-react-app capstone-app
cd capstone-app
```

2. Prepare the Runtime Injection
In public/index.html, add a script tag to the <head> to hold a placeholder string:

``` bash
<script>
  window.RUNTIME_CONFIG = {
    APP_ENV: "REPLACE_ME_APP_ENV"
  };
</script>
```

3. Consume the Variable
Update src/App.js to display the environment:

``` bash
function App() {
  const envName = window.RUNTIME_CONFIG?.APP_ENV || "local";
  return (
    <div style={{ textAlign: 'center', marginTop: '50px' }}>
      <h1>Welcome to the {envName} environment!</h1>
    </div>
  );
}
```

Phase 2: Containerization (The "One Image" Rule)
We build a single image that is configured via environment variables during startup.

I. The Entrypoint Script (entrypoint.sh)
This script uses sed to swap the placeholder in the compiled HTML with the actual $APP_ENV variable.

- Create the 'entrypoint.sh' file

``` bash
sudo nano entrypoint.sh
```
- Add the followwing content below and save.

``` bash
#!/bin/sh
sed -i "s/REPLACE_ME_APP_ENV/$APP_ENV/g" /usr/share/nginx/html/index.html
nginx -g 'daemon off;'
```

II. The Multi-Stage Dockerfile
We compile the app with Node and serve it with Nginx.
- Create the Dockerfile

``` bash
sudo nano Dockerfile
```
- Add the content below to the Dockerfile

``` bash
FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:stable-alpine
COPY --from=build /app/build /usr/share/nginx/html
COPY entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/entrypoint.sh
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
```


Phase 3: Environment Design & Deployment
Push the image to Docker Hub

- First create an account on DokckerHub - https://hub.docker.com/

- Login

``` bash
sudo docker login
```
- Build the app into a Docker Image

``` bash
sudo docker buidl -t your-dockerhub-user-name/react-app:v1 .
```

NB: Do not ignore the dot '.' at the end of the above command. It tells Docker to look in the current directory to build your Image.

- Push your image to DockerHub

``` bash
sudo docker push your-dockerhub-username/react-app:v1
```

Pull it onto your Ubuntu machine to simulate three distinct environments.

``` bash
sudo docker pull your-dockerhub-username/react-app:v1
```

``` bash
Environment         Port            Variable            Command Example
Dev                 8081            APP_ENV=dev         docker run -d -p 8081:80 -e APP_ENV=dev <image>
Staging             8082            APP_ENV=staging     docker run -d -p 8082:80 -e APP_ENV=staging <image>
Production          8083            APP_ENV=prod        docker run -d -p 8083:80 -e APP_ENV=prod <image>
```


If you have done this successfully, that is a big milestone! Seeing those three environments respond correctly on their respective ports means your Nginx + Entrypoint logic is working exactly as intended.

Now that our "v1" is stable, we move into the next phase of the capstone: 3. Deployment Simulation.

In a manual setup like this, "deploying an update" involves a specific cycle. Since we are using the "One Image" rule, we need to create a new version of that image (let's call it v2) and replace our running containers one by one.


The Workflow: The goal is to move a change from dev to staging to production without breaking everything at once.

- Modify: Make a visible change in your React code (e.g., change the background color or add "Version 2.0" to the text).

- Build & Push: Create the v2 image and push it to Docker Hub.

- Update Dev: Stop the container on port 8081 and start a new one using the v2 image.

- Repeat the process for staging and then production after verifying each step.


Documenting the "Pain": The project requires you to document downtime observed and risks encountered. As you do this manually, notice what happens between the moment you stop a container and the moment the new one is fully "Up."