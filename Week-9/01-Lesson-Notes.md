WEEK 9 — CI/CD Fundamentals
How Code Moves Safely from a Developer’s Laptop to Production

Week 9 Purpose

So far, you have:

- built Linux systems

- automated tasks

- containerized applications

- composed multi-service systems

But there is still a big unanswered question:

How do changes get deployed without breaking everything?

In the real world:

- code changes happen daily

- multiple engineers work together

- mistakes are inevitable

CI/CD exists to:

i. reduce human error

ii. automate testing

iii. make deployments repeatable

iv. protect production systems



1. The Problem CI/CD Solves (Real-World Context)

Without CI/CD

- Manual deployments

- Inconsistent steps

- “It worked yesterday”

- Production outages

- Stressful releases

With CI/CD

- Every change is tested

- Deployments follow the same steps

- Rollbacks are easier

- Production is safer

CI/CD turns deployment from an event into a routine.



2. Understanding CI vs CD (Plain Language)

Continuous Integration (CI)

- Developers push code frequently

- Code is automatically built, tested and validated.

Goal:

i. Catch problems early.

ii. Continuous Deployment (CD)

iii. Code that passes CI is packaged and deployed automatically.

Goal:

Release safely and consistently.


DevOps Thinking:
Humans write code.
Machines deploy it.



3. Tools in the CI/CD Ecosystem

We will start simple.

Common CI/CD tools:

i. GitHub Actions

ii. GitLab CI

iii. Jenkins

For this program, we’ll use GitHub Actions because:

- it’s beginner-friendly

- it integrates with GitHub

- it requires no extra servers



4. The CI/CD Pipeline Concept

A pipeline is a series of steps:

``` bash
Code Push
   ↓
Build
   ↓
Test
   ↓
Package
   ↓
Deploy
```

Each step must succeed before the next runs.



5. Your First CI Pipeline (Conceptual First)

When code is pushed:

- GitHub triggers a workflow

- Workflow runs defined steps

- Results are visible in GitHub

This is automation applied to software delivery.



6. Creating a GitHub Actions Workflow

Project structure
``` bash
.github/
└── workflows/
    └── ci.yml
```

ci.yml (Simple CI Example)
``` bash
name: CI Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Run basic checks
        run: |
          echo "Running CI checks"
          ls -la
```

Every push to main triggers this pipeline.

What Just Happened?

- GitHub spun up a Linux VM

- Checked out your code

- Ran commands automatically

This is real CI.