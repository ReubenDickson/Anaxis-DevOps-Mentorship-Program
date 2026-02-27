WEEK 12 REAL-WORLD PROJECT
Manually Scaling a Web Application and Observing Failure

Scenario: Your application is gaining users.

Management asks:

“Can we just run more containers?”

You are tasked with:

i. scaling manually

ii. observing system behavior

iii. identifying breaking points

iv. documenting operational pain


Project Tasks
1. Run Multiple Instances
``` bash
docker run -d -p 8081:80 my-app
docker run -d -p 8082:80 my-app
docker run -d -p 8083:80 my-app
```

Confirm each works:
``` bash
curl localhost:8081
curl localhost:8082
curl localhost:8083
```

2. Simulate Load

Repeatedly refresh or curl each endpoint.

Observe:

i. response times

ii. consistency

iii. user confusion

3. Simulate Failure

Stop one container:
``` bash
docker stop <container_id>
```

Observe:

i. which users are affected

ii. lack of automatic recovery

iii. manual intervention required


4. Attempt Manual Traffic Control

Manually redirect traffic (conceptually or with notes).

Document:

i. how difficult this is

ii. what could go wrong

iii. how this scales poorly


Reflection (Mandatory)

Questions you must answer:

i. What broke first?

ii. What was hardest to manage?

iii. Why is manual scaling unsafe?

iv. What happens with 10 or 50 instances?

v. What is clearly missing?

STUDENT DELIVERABLES
``` bash
week12-scaling-project/
├── documentation/
│   ├── scaling_attempt.md
│   ├── failure_observed.md
│   ├── availability_gaps.md
│   ├── operational_pain.md
│   └── conclusions.md
├── screenshots/
│   └── running-instances.png
└── README.md
```