# KUBERNETES_VETTEC_20230313

kubectl get nodes - List all nodes

kubectl get all --all-namespaces - List all resources in all namespaces

kubectl run getting-started --image bjgomes/getting-started - Create a deployment with the name getting-started

kubectl get pods - List all pods

kubectl describe pod <name of pod>

kubectl edit pod <name of pod>

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: getting-started
spec:
  containers:
  - name: getting-started
    image: bjgomes/getting-started
    ports:
    - containerPort: 80
```