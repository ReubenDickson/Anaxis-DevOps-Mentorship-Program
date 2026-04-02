WEEK 14 — Why Kubernetes Exists & Core Concepts: From Containers to Orchestration

Week 14 Purpose

Before learning any commands, you must understand:

- Why Docker alone fails at scale
- What orchestration means
- What “desired state” really is
- Why clusters exist
- Why pods are not containers

This week is conceptual clarity.

``` bash
___
```

1. What Is Orchestration?

Running containers is like playing one instrument while orchestration is conducting an orchestra.

When you have:

- multiple containers
- multiple nodes
- dynamic scaling
- failures happening constantly

You need coordination.

Kubernetes is that coordinator.

DevOps Thinking: Containers solve packaging.
Kubernetes solves coordination.

``` bash
___
```

2. Kubernetes in One Sentence

Kubernetes is a system that ensures your applications are always running in the state you declare.

Not:

- a deployment tool
- a container runtime
- a replacement for Docker

It is a state management system for containerized workloads.

``` bash
___
```

3. The Desired State Concept

This is the most important idea in Kubernetes.

You do not tell Kubernetes:

“Start 3 containers.”

You tell it:

“I want 3 replicas running at all times.”

If one dies:

Kubernetes replaces it automatically.

You define what should be true.
Kubernetes ensures it becomes and stays true.

``` bash
___
```

4. Cluster Architecture (High Level)

A Kubernetes cluster consists of:

A. Control Plane which is the brain;

i. Scheduler

ii. API Server

iii. Controller Manager

iv. etcd

B. Worker Nodes

This is where applications actually run.

``` bash
___
```

5. What Is a Pod?

A Pod:

- is the smallest deployable unit
- contains one or more containers
- shares networking and storage

Important: Kubernetes manages Pods, not containers directly.

``` bash
___
```

6. Why Pods Matter

In Docker:

You manage containers manually.

In Kubernetes:

Pods are created by controllers.
Pods are replaced automatically if they fail.

This removes manual intervention.



Week 14 Practical Setup

We introduce Kubernetes using:

Minikube (local cluster)
or
Kind (Kubernetes in Docker)

Example (Minikube):
``` bash
minikube start
kubectl get nodes
```

Verify:

- cluster is running
- node exists

Minikube is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes.
See Minikube documentation here: https://minikube.sigs.k8s.io/docs/start/?arch=%2Fwindows%2Fx86-64%2Fstable%2F.exe+download