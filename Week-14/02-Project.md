Week 14 Mini Project

Deploy a simple Nginx pod:
``` bash
kubectl run nginx --image=nginx
kubectl get pods
```

Now Delete it:
``` bash
kubectl delete pod nginx
```
Observe: It does NOT restart automatically.

This sets up Week 15, where Deployments fix this.