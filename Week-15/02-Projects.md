WEEK 15 REAL-WORLD PROJECT

Self-Healing Web Application
Scenario: You must deploy a web application that:

- runs multiple instances
- survives container failure
- exposes a stable endpoint
- distributes traffic automatically

Project Tasks
1. Create Deployment: 3 replicas of nginx.

2. Verify Pods
``` bash
kubectl get pods
```

3. Simulate Failure: Delete one Pod.

Observe automatic replacement.

4. Expose Service: Create NodePort Service.

5. Access Application
``` bash
minikube service nginx-service
```
or
``` bash
NodeIP:NodePort
```

6. Scale Application
``` bash
kubectl scale deployment nginx-deployment --replicas=5
```
Observe new Pods created automatically.


Project Implementation

Phase 1: Environment & Directory Setup
Before applying manifests, organize your workspace.

``` bash
mkdir -p week15-kubernetes-project/{manifests,documentation,screenshots}
cd week15-kubernetes-project
```

1. The Deployment Manifest (manifests/deployment.yaml)
Create this file and add the following content;

``` bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
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
        image: nginx:latest
        ports:
        - containerPort: 80
```

2. The Service Manifest (manifests/service.yaml)
While you used kubectl expose earlier, in a professional project, you should always use a declarative YAML file.

``` bash
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 32223
```

Phase 2: Project Execution Steps

Step 1: Deploy & Verify: Apply both files to your Minikube cluster.

``` bash
kubectl apply -f manifests/deployment.yaml
kubectl apply -f manifests/service.yaml
```

Verify: You should see 3 replicas running.
``` bash
kubectl get pods
```
``` bash
kubectl get service
```
You should see nginx-service with the NodePort 32223.

Step 2: Test Self-Healing (The "Chaos" Test)

This is the heart of the project. Kubernetes' Replication Controller constantly monitors the cluster to ensure the actual state matches your desired state (3 replicas).

Pick a random pod:
``` bash
kubectl get pods
```

Kill it:
``` bash
kubectl delete pod <POD_NAME>
```

Immediately watch and observe how kubernetes creates another pod instantly to replace the dead pod:
``` bash
kubectl get pods -w
```

Observation: You will see a new pod being "ContainerCreating" the moment the old one is "Terminating."

Step 3: Accessing & Scaling
Since you are on Minikube, use the helper command to open the browser:

``` bash
minikube service nginx-service
```

Scale to 5 Replicas:

Update your deployment.yaml (change replicas: 3 to 5) and run: 
``` bash
kubectl apply -f manifests/deployment.yaml.
```

Why? Always prefer updating the YAML over using kubectl scale for documentation, as it maintains a "Source of Truth."


Reflection Questions

Answer the following questions:

i. What problem do Deployments solve?

ii. Why are Pods not managed directly?

iii. How does Kubernetes maintain availability?

iv. Why are Services necessary?

v. What operational problems disappeared compared to Phase 3?


Submission Structure

``` bash
📁 
week15-kubernetes-project/
├── manifests/
│   ├── deployment.yaml
│   └── service.yaml
├── documentation/
│   ├── self_healing_test.md
│   ├── scaling_test.md
│   └── networking_explanation.md
├── screenshots/
│   ├── pods_running.png
│   └── service_access.png
└── README.md
```