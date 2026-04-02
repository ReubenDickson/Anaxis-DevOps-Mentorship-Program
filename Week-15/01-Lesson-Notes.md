WEEK 15 — Deployments, Services & Self-Healing: How Kubernetes Keeps Applications Running

Week 15 Purpose

In Phase 3 students experienced:

- containers dying
- downtime during deployments
- manual scaling chaos
- port confusion
- traffic management problems

Week 15 introduces the first real orchestration primitives that solve those problems.

You will learn:

i. how Kubernetes maintains running applications

ii. how Kubernetes replaces failed instances

iii. how Kubernetes provides stable networking

iv. how applications become self-healing

This week transforms Kubernetes from interesting to essential.

``` bash
1. Why Pods Alone Are Not Enough
```

A Pod represents one running instance.

If the Pod dies, Kubernetes does nothing and the application disappears.

This is not acceptable in production.

Real systems require:

i. automatic restarts

ii. automatic replacement

iii. scaling management

This is where Deployments enter.


DevOps Thinking: Pods run applications. Deployments manage Pods.


``` bash
2. What Is a Deployment?
```

A Deployment is a Kubernetes object that ensures:

- a specified number of Pods exist
- Pods are replaced if they fail
- updates happen in a controlled way

Instead of running Pods directly, engineers declare: “I want 3 replicas of this application.”

Kubernetes guarantees it.

``` bash
3. ReplicaSets — The Hidden Worker
```

Deployments internally create ReplicaSets.

ReplicaSets ensure:

i. the correct number of Pods exist

ii. missing Pods are recreated

iii. scaling changes are applied

You usually interact with Deployments, not ReplicaSets directly.

DevOps Thinking: ReplicaSets enforce replica count. Deployments manage application lifecycle.

``` bash
4. Creating a Deployment named 'nginx-deployment.yaml'
```
Example deployment:

``` bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```
Apply it:
``` bash
kubectl apply -f nginx-deployment.yaml
```
Check:
``` bash
kubectl get deployments
kubectl get pods
```

You should see 3 Pods created automatically.

``` bash
5. Self-Healing Demonstration
```

Delete a Pod:
``` bash
kubectl delete pod <pod-name>
```

Observe:
``` bash
kubectl get pods
```

A new Pod appears automatically.

Why?

Because the Deployment declared:

replicas: 3

Kubernetes restores desired state.


DevOps Thinking: You do not fix failures manually. You design systems that heal themselves.

``` bash
6. The Networking Problem
```
In Phase 3 we saw scaling problems:

- multiple containers
- different ports
- user confusion
- manual traffic routing

Kubernetes solves this with Services.

``` bash
7. What Is a Service?
```
A Service provides:

i. a stable endpoint

ii. traffic distribution

iii. internal discovery

Even if Pods restart, move or scale, the Service address remains stable.

``` bash
8. Creating a Service
```
Example:
``` bash
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
```

Apply:
``` bash
kubectl apply -f nginx-service.yaml
```

Now traffic flows through the Service to Pods.

``` bash
9. Service Types (Simplified)
```

``` bash
Type            Purpose
ClusterIP       Internal communication
NodePort        Expose service via node port
LoadBalancer    External cloud load balancer
```

For learning environments we often use NodePort.

DevOps Thinking: Pods are temporary. Services provide stability.