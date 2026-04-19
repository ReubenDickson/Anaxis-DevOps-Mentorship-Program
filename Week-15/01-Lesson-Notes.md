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

9. View Running Pods Using Port Forwarding (Temporal)
This creates a temporary tunnel from your computer directly to a specific Pod. Great for debuggung, but this is a temporay solution.

``` bash
kubectl port-forward pod-name 8080:80
```

Now view running pod on your browser - localhost:8080
You can repeat this for other pods using 8081 and 8082 respectively. However in a production environment with probably thousands of pods, that would be a lot of manual work, and that is where Service comes in.


10. Service Types (Simplified)

``` bash
Type            Purpose
ClusterIP       Internal communication
NodePort        Expose service via node port
LoadBalancer    External cloud load balancer
```

A Service acts as a single front door (one IP or DNS name). When traffic hits that door, the service automatically balances the load across all pods in your deployment. If one pod goes down, the Service just sends traffic to the healthy ones.

ClusterIP (The Internal Door)
This is the default type. It gives the Service an IP address that is only reachable from within the cluster. It is best for databases or internal microservices that shouldn't be exposed to the public internet.

NodePort (The External Side-Door)
This opens a specific port (usually between 30000-32767) on every single Node (server) in your cluster. If you visit "NodeIP:Port", you will reach your app. This is best for testing or in evironments where you don't need a load balancer.

Load Balancer (The Front Gate)
This is the "pro" way to do it. If you're on a cloud provider (like AWS or Azure), Kubernetes will talk to the cloud API and provision a real public IP address for you. This is best for production websites and public-facing applications.

``` bash
kubectl get deployments
kubectl expose deployment nginx-deployment --type=NodePort --port=80
kubectl get service
```

Confirm that your Service can find your Pods, you should see a list of IP addresses.
``` bash
kubectl get endpoints nginx-service
```

Now see your app running on your browser by following the URL from the command below.
``` bash
minikube service nginx-service
```

For learning environments we often use NodePort.

DevOps Thinking: Pods are temporary. Services provide stability.