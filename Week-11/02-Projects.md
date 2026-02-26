WEEK 11 REAL-WORLD PROJECT
Simulating Downtime and Practicing Safe Deployment Thinking

Scenario: You manage a production-like environment running a web application in Docker.

Your task is to:

i. simulate deployment-induced downtime

ii. observe user impact

iii. attempt safer deployment approaches

iv. document what improves reliability


Project Tasks
1. Baseline Deployment

Run a web container:
``` bash
docker run -d -p 8080:80 my-app
```
Confirm:

``` bash
curl localhost:8080
```

2. Simulate Downtime (Recreate Strategy)
``` bash
docker stop <container_id>
docker rm <container_id>
docker run -d -p 8080:80 my-app-v2
```
Observe:

i. downtime duration

ii. user experience


3. Attempt a Safer Deployment

Run second container on different port:
``` bash
docker run -d -p 8081:80 my-app-v2
```
Manually switch traffic: stop old container only after new is running

Observe:

i. reduced downtime

ii. improved reliability


4. Rollback Simulation

Stop new container.
Restart old version.

Document:

i. how fast rollback occurred

ii. what failed safely


Reflection (Mandatory)

Questions you must answer:

i. What caused downtime?

ii. Which strategy reduced risk?

iii. What still felt fragile?

iv. Why is manual traffic switching unreliable?

v. What is missing from this setup?

📁 STUDENT DELIVERABLES
``` bash
week11-reliability-project/
├── documentation/
│   ├── downtime_observed.md
│   ├── deployment_strategy_comparison.md
│   ├── rollback_experience.md
│   └── lessons_learned.md
├── screenshots/
│   └── container-states.png
└── README.md
```