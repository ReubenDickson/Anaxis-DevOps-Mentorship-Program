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