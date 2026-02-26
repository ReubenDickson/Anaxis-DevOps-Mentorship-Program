WEEK 10 — Application Deployment & Environment Management
What “Deployment” Really Means in the Real World

Week 10 Purpose

Before this week, you have:

- built systems

- containerized applications

- automated builds

- run services successfully

But running an application is not the same thing as deploying one.

This week answers a fundamental DevOps question:

What actually happens when software is released to users?

You will learn:

i. what deployment truly means

ii. why environments exist

iii. why “just updating the container” can break users

iv. how configuration drift happens

v. why DevOps engineers treat production differently

This week reshapes how you think about change.

1. What Is Deployment? (Correct Definition)

Deployment is the controlled process of making a new version of software available to users.

Deployment is NOT:

- copying files

- restarting containers

- running docker run

- pushing code blindly

Deployment IS a planned change with user impact that must be safe, reversible, and repeatable.

DevOps engineers think in deployment events, not commands.


DevOps Thinking

Every deployment is a risk.
The job is to control that risk.



2. Why Environments Exist

Real systems never have just one environment.

At minimum, there are three:
``` bash
Environment                    Purpose
1. Development (dev)           Where changes are built
2. Staging                     Where changes are tested
3. Production (prod)           Where real users exist
```
Why this matters
i. Bugs are caught before users see them
ii. Configuration differs per environment

Mistakes in dev must never reach prod directly.


3. Environment ≠ Code (Critical Concept)

A common beginner mistake: “If the code is the same, the environment doesn’t matter.”

This is false.

Environments differ by:
i. ports

ii. database URLs
iii. API keys
iv. logging levels
v. resource limits

This is called configuration drift.

DevOps Thinking:
Code should not change per environment.
Configuration should.


4. Externalizing Configuration

In DevOps, configuration must be:

i. outside the application
ii. injected at runtime
iii. environment-specific

Example (Docker)
``` bash
docker run -e APP_ENV=production -e LOG_LEVEL=info my-app
```
Same image.
Different behavior.

This is how real deployments work.


5. Manual Deployment: What Can Go Wrong

Let’s examine a naive deployment process:

- You stop a running container

- You build new image

- Then you start new container

What breaks?

i. users experience downtime

ii. failure mid-deployment causes outage

iii. rollback is unclear

iv. no audit trail

This is why DevOps engineers plan deployments, not rush them.



6. Deployment Responsibility Shift

Before deployment, You are a builder

During deployment, You are responsible for users

After deployment, You are responsible for stability

This is the mental shift Phase 3 enforces.