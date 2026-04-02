WEEK 16 — Configuration, Secrets & Resource Management: Managing Application Settings and System Resources in Kubernetes

Week 16 Purpose

By now you have seen Kubernetes:

- run applications as Deployments
- maintain replica counts automatically
- expose stable endpoints via Services

However, real applications require more:

- environment-specific configuration
- secure handling of sensitive data
- limits on CPU and memory usage
- the ability to scale responsibly

Week 16 introduces these capabilities and connects them directly to operational discipline.

``` bash
1. Configuration in Distributed Systems
```
Applications rarely run with identical settings everywhere.

Examples of configuration differences:
``` bash
Setting                 Dev                 Production
Logging level	        debug               warning
Database host	        localhost	        production-db
Feature flags	        enabled             disabled
API endpoints	        staging             live
```
Hardcoding these values inside container images is dangerous and inflexible.

DevOps engineers separates application Code from application Configuration.

``` bash
2. ConfigMaps — Externalizing Configuration
```

A ConfigMap stores non-sensitive configuration data for applications.

Instead of rebuilding images, configuration can be injected dynamically.

Example ConfigMap:
``` bash
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: production
  LOG_LEVEL: info
```

Apply:
``` bash
kubectl apply -f configmap.yaml
```

``` bash
3. Using ConfigMaps in Pods
```
ConfigMaps can be injected as environment variables.

Example in Deployment:
``` bash
env:
  - name: APP_ENV
    valueFrom:
      configMapKeyRef:
        name: app-config
        key: APP_ENV
```

Now configuration changes do not require rebuilding the image.

This is critical for managing multiple environments.


DevOps Thinking: Applications should be portable. Configuration should be replaceable.

``` bash
4. Secrets — Protecting Sensitive Information
```
Not all configuration is safe to expose.

Sensitive values include:

- database passwords
- API tokens
- private keys
- encryption credentials

Kubernetes stores these using Secrets.

Example Secret:
``` bash
kubectl create secret generic db-secret \
  --from-literal=password=mysecurepassword
```

Secrets can then be injected into Pods just like ConfigMaps.

``` bash
5. Why Secrets Matter
```
Without proper secret handling:

i. passwords appear in code repositories

ii. credentials leak in logs

iii. environments become insecure

Secrets management is fundamental to secure DevOps practices.

``` bash
6. Resource Requests and Limits
```
Containers must share cluster resources.

Without limits:

- one container can consume all CPU
- memory exhaustion can crash nodes
- workloads become unpredictable

Kubernetes allows defining resource boundaries.

Example:
``` bash
resources:
  requests:
    memory: "64Mi"
    cpu: "250m"
  limits:
    memory: "128Mi"
    cpu: "500m"
Requests
```

Minimum resources required.

Limits

Maximum resources allowed.


DevOps Thinking: Resources must be allocated intentionally.

Uncontrolled resource consumption leads to instability.

``` bash
7. Horizontal Scaling
```

Earlier, we scaled Deployments manually:
``` bash
kubectl scale deployment nginx-deployment --replicas=5
```

In real systems, scaling often responds to load.

Kubernetes provides Horizontal Pod Autoscaling (HPA) to adjust replicas automatically.

Conceptually:

Higher CPU usage → more Pods
Lower CPU usage → fewer Pods

For this program we introduce the concept, but full automation may be explored later.