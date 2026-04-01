WEEK 9 REAL-WORLD PROJECT
“CI Pipeline for a Containerized Application”

Scenario: Your team maintains a Dockerized web application.

They want:

i. automatic validation on every code push

ii. confidence that Docker builds won’t break

You are tasked with setting up a CI pipeline.


Project Tasks
1. Repository Setup

Use your Week 7 or 8 Docker project.

2. Create CI Workflow
Inside your Week 7 or 8 project, create a new .github/workflows/docker-ci.yml

``` bash
name: Docker CI

on:
  push:
    branches:
      - main

jobs:
  docker-build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Build Docker image
        run: docker build -t test-image .

      - name: Run container
        run: docker run -d test-image
```

3. Trigger the Pipeline
``` bash
git init
git add .
git config --global user.email "your-email-address"
git config --global user.name "Your Name"
git commit -m "Add CI pipeline"
git branch -M main
git remote add origin git@github.com:your-git-username/your-repository-name.git
git push -u origin main
```

4. Verify Results

Go to GitHub → Actions

Confirm pipeline ran successfully.