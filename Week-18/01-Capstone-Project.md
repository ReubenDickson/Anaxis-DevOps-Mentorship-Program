WEEK 18 — Phase 4 Capstone: Operating a Kubernetes-Based System: Deploying, Scaling, Updating, and Recovering a Real Application

Week 18 Purpose

Throughout Phase 4 you learned:

- Kubernetes architecture (Week 14)
- Deployments, ReplicaSets, Services (Week 15)
- ConfigMaps, Secrets, Resource management (Week 16)
- Rolling updates, probes, and rollback (Week 17)

Week 18 combines all these concepts into a real operational scenario.

The goal is to simulate what a DevOps engineer does daily:

i. deploy applications

ii. manage configuration

iii. handle scaling

iv. monitor health

v. recover from failure

vi. update systems safely

You must now operate the system responsibly.


Capstone Scenario: You are part of a small DevOps team responsible for deploying a containerized web application on Kubernetes.

The application must:

i. run multiple instances

ii. remain available during updates

iii. load configuration externally

iv. store sensitive values securely

v. recover automatically if containers fail

vi. scale when traffic increases

Management requires:

i. reliability

ii. maintainability

iii. documentation of operational procedures


CAPSTONE PROJECT REQUIREMENTS

You must build a complete Kubernetes deployment architecture.

1. Application Deployment

Create a Deployment that:

i. runs 3 replicas

ii. uses nginx or a simple web application

iii. includes resource requests and limits

Example concept:

``` bash
replicas: 3
resources:
  requests:
    cpu: "250m"
    memory: "64Mi"
  limits:
    cpu: "500m"
    memory: "128Mi"
```

2. Service Exposure

Create a Service that:

i. exposes the application externally

ii. distributes traffic across replicas

iii. remains stable even if Pods restart

Use NodePort for local environments.


3. ConfigMap Integration

Create a ConfigMap that provides application configuration such as:

``` bash
APP_ENV=production
APP_VERSION=v1
APP_NAME=DevOpsBootcampApp
```
Deployment must load these variables.

4. Secret Management

Create a Secret that simulates sensitive data:

i. database password

ii. API key

iii. authentication token

Inject the secret into the container environment.


5. Health Monitoring

Deployment must include:

i. Liveness Probe

ii. Readiness Probe

This ensures failed containers restart automatically and traffic only reaches healthy Pods.


6. Rolling Update Simulation

You must perform a rolling update by:

i. changing the application version

ii. observing the rollout process

Command example:
``` bash
kubectl set image deployment/web-app web=nginx:latest
```
Observe:
``` bash
kubectl rollout status deployment/web-app
```

7. Failure Simulation

You must simulate failures by:

i. deleting Pods

ii. breaking configuration temporarily

iii. observing Kubernetes recovery

Command example:
``` bash
kubectl delete pod <pod-name>
```
Expected behavior:
``` bash
Kubernetes automatically recreates the Pod.
```

8. Rollback Simulation

You must deploy a faulty version and then rollback.

Example:
``` bash
kubectl rollout undo deployment/web-app
```

This demonstrates safe production recovery.

9. Scaling Simulation

You must scale the application:
``` bash
kubectl scale deployment web-app --replicas=6
```

Observe:
``` bash
kubectl get pods
```
Verify that new Pods are created and traffic continues flowing.


Capstone Reflection

You must write detailed reflections answering:

i. How Kubernetes solved the scaling problems from Phase 3

ii. How rolling updates prevent downtime

iii. Why health probes are critical

iv. What happens if configuration is mismanaged

v. Why orchestration becomes necessary in production

Reflection is essential to reinforce engineering thinking.

``` bash
📁 Submission Structure
week18-kubernetes-capstone/
├── manifests/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── configmap.yaml
│   └── secret.yaml
│
├── documentation/
│   ├── architecture_design.md
│   ├── deployment_steps.md
│   ├── failure_simulation.md
│   ├── scaling_test.md
│   └── lessons_learned.md
│
├── screenshots/
│   ├── pods_running.png
│   ├── rollout_update.png
│   └── scaling_results.png
│
└── README.md
```

Phase 4 Learning Outcomes

After completing Phase 4, students can confidently:

i. operate Kubernetes clusters locally

ii. deploy containerized applications

iii. scale applications safely

iv. manage configuration and secrets

v. perform rolling updates and rollbacks

vi. understand how orchestration solves real production problems

At this stage you are no longer beginners.

You now understand modern infrastructure fundamentals.


So, Where We Are in the 6-Month Program,..

Completed:

Phase 1 — Linux Foundations
Phase 2 — Containers & CI/CD
Phase 3 — Deployment, Reliability & Scaling
Phase 4 — Kubernetes Foundations

Remaining:

Phase 5 — Cloud Platforms & Infrastructure

(Weeks 19–22)

Phase 6 — Production DevOps, Observability & Final Capstone

(Weeks 23–26)


Phase 5 will move the system from local clusters to real infrastructure.

You will learn:

i. Cloud fundamentals

ii. Infrastructure provisioning

iii. Networking in the cloud

iv. Load balancing

v. Managed services

vi. Infrastructure as Code (Terraform introduction)