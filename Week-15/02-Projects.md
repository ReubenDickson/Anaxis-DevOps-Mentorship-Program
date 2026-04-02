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


Reflection Questions

Answer the following questions:

i. What problem do Deployments solve?

ii. Why are Pods not managed directly?

iii. How does Kubernetes maintain availability?

iv. Why are Services necessary?

v. What operational problems disappeared compared to Phase 3?

``` bash
📁 Submission Structure
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