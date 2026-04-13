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


Step-by-step Process to Install and Get Started With Minikube

System Requirements

``` bash
Minimum Specs
CPU: 2 cores
RAM: 4GB (recommended 8GB)
Disk: 20GB free
OS: Ubuntu 22.04 / 24.04
```

Recommended Setup (Docker Driver)

Step 1 — Install Docker (You probably already have Docker installed)

``` bash
sudo apt update
sudo apt install -y docker.io
```

Start and enable:
``` bash
sudo systemctl enable docker
sudo systemctl start docker
```

Add user to docker group:
``` bash
sudo usermod -aG docker $USER
newgrp docker
```

Verify:
``` bash
docker run hello-world
```

Step 2 — Install kubectl

``` bash
curl -LO https://dl.k8s.io/release/v1.35.1/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```
Verify:

``` bash
kubectl version --client
```

Step 3 — Install Minikube
``` bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

Verify:
``` bash
minikube version
```

Step 4 — Start Minikube (Docker Driver)
``` bash
minikube start --driver=docker --memory=4096 --cpus=2
```

Step 5 — Verify Cluster
``` bash
kubectl get nodes
kubectl get pods -A
```
Expected:
``` bash
minikube   Ready
```

Step 6 — Enable Essential Addons
``` bash
minikube addons enable storage-provisioner
minikube addons enable default-storageclass
```

Step 7 — Test Deployment
``` bash
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --type=NodePort --port=80
minikube service nginx
```