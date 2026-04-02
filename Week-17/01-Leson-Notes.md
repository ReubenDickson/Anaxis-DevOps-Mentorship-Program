WEEK 17 — Rolling Updates, Rollbacks & Health Checks: Achieving Safe, Zero-Downtime Deployments

Week 17 Purpose

Real systems evolve constantly.

New versions of applications must be deployed while:

- users are actively using the system
- traffic is flowing continuously
- failures can occur at any time

Stopping a system to deploy updates is unacceptable in modern infrastructure.

Week 17 introduces Kubernetes mechanisms that make safe continuous delivery possible:

- Rolling Updates
- Automatic Rollbacks
- Liveness Probes
- Readiness Probes

These mechanisms ensure applications remain available during change.

``` bash
1. The Deployment Problem
```
Imagine you have 5 application instances running.

If you stop all of them to deploy a new version:

Old Version → Stopped
New Version → Starting
Users → Receive errors

This causes downtime.

Kubernetes solves this using rolling updates.

``` bash
2. Rolling Updates
```

A Rolling Update gradually replaces old Pods with new ones.

Example process:

Pod v1  → replaced by Pod v2
Pod v1  → replaced by Pod v2
Pod v1  → replaced by Pod v2

At every step, some Pods remain available, traffic continues flowing and Users do not experience downtime.


DevOps Thinking: Changes must be incremental, not disruptive.

Example Rolling Update

Update image version in Deployment:
``` bash
containers:
  - name: web
    image: nginx:1.25
```
Apply:
``` bash
kubectl apply -f deployment.yaml
```
Observe rollout:
``` bash
kubectl rollout status deployment nginx-deployment
```
Pods are replaced gradually.

``` bash
3. Rollbacks
```

Even well-tested deployments can fail.

If a deployment introduces problems, Kubernetes allows instant rollback.

Example:
``` bash
kubectl rollout undo deployment nginx-deployment
```

Kubernetes restores the previous working version.

DevOps Thinking: Every deployment must have a clear rollback path.

``` bash
4. Why Health Checks Matter
```
Suppose a Pod starts successfully but the application inside it is broken.

Without health checks:

- Kubernetes thinks the Pod is healthy
- traffic is sent to a broken service
- users experience failures

To prevent this, Kubernetes uses probes.

``` bash
5. Liveness Probes
```
A Liveness Probe answers: Is the application still alive?

If the probe fails:

Kubernetes restarts the container automatically.

Example:
``` bash
livenessProbe:
  httpGet:
    path: /
    port: 80
  initialDelaySeconds: 10
  periodSeconds: 5
  ```

``` bash
6. Readiness Probes
```
A Readiness Probe answers: Is the application ready to receive traffic?

If readiness fails:

- the Pod remains running
- but Kubernetes stops sending traffic to it

This prevents users from hitting half-initialized services.

Example:
``` bash
readinessProbe:
  httpGet:
    path: /
    port: 80
  initialDelaySeconds: 5
  periodSeconds: 5
```

DevOps Thinking: Applications must prove they are healthy before serving users.


``` bash
7. Combining Probes with Rolling Updates
```
The deployment process becomes safer:

- New Pod starts
- Readiness probe verifies it
- Traffic shifts to it
- Old Pod is terminated

This guarantees smooth transitions between versions.