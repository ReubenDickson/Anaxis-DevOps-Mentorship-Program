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
Inside your Week 7 or 8 project, create a new
.github/workflows/docker-ci.yml

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

3️⃣ Trigger the Pipeline
git add .
git commit -m "Add CI pipeline"
git push origin main

4️⃣ Verify Results

Go to GitHub → Actions

Confirm pipeline ran successfully

🧠 HOW A DEVOPS ENGINEER THINKS (WEEK 9)

Every change is risky

Automation reduces risk

CI is mandatory, not optional

Pipelines must be simple and reliable

If it’s not automated, it’s not scalable

📁 STUDENT DELIVERABLES
week9-cicd-project/
├── .github/workflows/
│   └── docker-ci.yml
├── Dockerfile
├── README.md
└── screenshots/
    └── pipeline-success.png


README must explain:

what CI/CD is

what the pipeline does

why automation matters

✅ WHAT WEEK 9 GIVES STUDENTS

By the end of Week 9, students:

understand CI/CD concepts clearly

build a working pipeline

automate Docker builds

read pipeline logs

think in delivery workflows

🔜 Next (Week 10 Options)

We can now proceed to:

Kubernetes fundamentals

CD deployment strategies

Cloud-based CI/CD

Infrastructure as Code (Terraform intro)

Tell me how you want Week 10 structured, and we continue.