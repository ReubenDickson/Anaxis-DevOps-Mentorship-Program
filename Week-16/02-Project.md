WEEK 16 REAL-WORLD PROJECT

Configurable & Resource-Controlled Web Service

Scenario: You must deploy a web application that:

- reads configuration from a ConfigMap
- retrieves sensitive credentials from a Secret
- runs with controlled CPU and memory usage
- can scale when needed

Project Tasks
1. Create ConfigMap

Include variables such as:
``` bash
APP_ENV
APP_NAME
LOG_LEVEL
```

2. Create Secret

Simulate database credentials.

3. Update Deployment

Deployment must:

- use ConfigMap values
- inject Secret values
- include resource requests and limits

4. Test Deployment

Verify:
``` bash
kubectl describe pod
```

Confirm environment variables and resources.

5. Scale Deployment

Manually increase replicas and observe behavior.


Reflection Questions

Answer the following questions:

i. Why should configuration be externalized?

ii. What risks arise from hardcoded credentials?

iii. What happens if resource limits are not defined?

iv. Why is scaling important for reliability?

``` bash
📁 Submission Structure
week16-kubernetes-project/
├── manifests/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── configmap.yaml
│   └── secret.yaml
├── documentation/
│   ├── configuration_strategy.md
│   ├── secrets_explanation.md
│   └── resource_management.md
├── screenshots/
│   └── pod_details.png
└── README.md

```