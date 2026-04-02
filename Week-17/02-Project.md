WEEK 17 REAL-WORLD PROJECT
Zero-Downtime Deployment Simulation

Scenario: Your team must deploy a new version of a web application without interrupting user access.

The deployment must:

i. update Pods gradually

ii. maintain service availability

iii. detect unhealthy containers

iv. support quick rollback


Project Tasks
1. Create Deployment

Start with 3 replicas of nginx.

2. Add Health Probes

Include both:

i. readiness probe

ii. liveness probe

3. Perform a Rolling Update

Update the image version:
``` bash
kubectl set image deployment/nginx-deployment nginx=nginx:latest
```
Observe rollout:
``` bash
kubectl rollout status deployment nginx-deployment
```

4. Simulate Failure

Deploy a broken version intentionally.

Observe:

- failed Pods
- probe failures
- recovery behavior

5. Rollback
``` bash
kubectl rollout undo deployment nginx-deployment
```
Confirm system stability returns.

Reflection Questions

Students must answer:

i. Why are rolling updates safer than restarts?

ii. What problems do readiness probes prevent?

iii. What happens if liveness probes are absent?

iv. Why is rollback capability essential in production?

``` bash
📁 Submission Structure
week17-kubernetes-project/
├── manifests/
│   ├── deployment.yaml
│   └── service.yaml
├── documentation/
│   ├── rolling_update_process.md
│   ├── probe_configuration.md
│   └── rollback_test.md
├── screenshots/
│   └── rollout_status.png
└── README.md
```