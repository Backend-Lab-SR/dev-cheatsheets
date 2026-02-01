# Kubernetes Cheatsheet

## Basic Commands

```bash
# Check cluster status
kubectl cluster-info
kubectl get nodes

# Get Kubernetes version
kubectl version --short
kubectl version --client --short

# View configuration
kubectl config view
kubectl config current-context
kubectl config get-contexts

# Switch context
kubectl config use-context <context-name>
```

## Pod Management

```bash
# List pods
kubectl get pods
kubectl get pods -A  # All namespaces
kubectl get pods -n <namespace>
kubectl get pods -o wide  # More details

# Describe pod
kubectl describe pod <pod-name>
kubectl describe pod <pod-name> -n <namespace>

# Get pod logs
kubectl logs <pod-name> --previous
# Logs with namespace
kubectl logs <pod-name> -n <namespace>
kubectl logs -f <pod-name>  # Follow logs
kubectl logs --tail=100 <pod-name>
kubectl logs <pod-name> -c <container-name>  # Multi-container

# Execute command in pod
kubectl exec -it <pod-name> -- <command>
kubectl exec -it <pod-name> -- /bin/bash

# Delete pod
kubectl delete pod <pod-name>
# WARNING: Force delete can cause data loss
kubectl delete pod <pod-name> --force --grace-period=0

# Port forward
kubectl port-forward <pod-name> 8080:80
kubectl port-forward <pod-name> 8080:80 -n <namespace>
```

## Deployment Management

```bash
# List deployments
kubectl get deployments
kubectl get deploy

# Create deployment
kubectl create deployment <name> --image=<image>
kubectl apply -f deployment.yaml

# Scale deployment
kubectl scale deployment <name> --replicas=3
kubectl scale --replicas=3 deployment/<name>

# Update deployment
kubectl set image deployment/<name> <container>=<image>
kubectl rollout restart deployment/<name>

# Rollout status
kubectl rollout status deployment/<name>
kubectl rollout history deployment/<name>

# Pause / resume rollout
kubectl rollout pause deployment/<name>
kubectl rollout resume deployment/<name>

# Rollback deployment
kubectl rollout undo deployment/<name>
kubectl rollout undo deployment/<name> --to-revision=2

# Delete deployment
kubectl delete deployment <name>
```

## Service Management

```bash
# List services
kubectl get services
kubectl get svc

# Create service
kubectl create service <type> <name> --tcp=8080:80
kubectl apply -f service.yaml

# Expose deployment as service
kubectl expose deployment <name> --port=80 --target-port=8080

# Describe service
kubectl describe service <name>

# Delete service
kubectl delete service <name>
```

## Namespace Management

```bash
# List namespaces
kubectl get namespaces
kubectl get ns

# Create namespace
kubectl create namespace <name>

# Set default namespace
kubectl config set-context --current --namespace=<name>

# Delete namespace
kubectl delete namespace <name>

# Get resources in namespace
kubectl get all -n <namespace>
```

## ConfigMap and Secrets

```bash
# List configmaps
kubectl get configmaps
kubectl get cm

# Create configmap
kubectl create configmap <name> --from-literal=key=value
kubectl create configmap <name> --from-file=<file>
kubectl apply -f configmap.yaml

# Describe configmap
kubectl describe configmap <name>

# List secrets
kubectl get secrets

# Create secret
kubectl create secret generic <name> --from-literal=key=value
kubectl create secret generic <name> --from-file=<file>
kubectl create secret docker-registry <name> --docker-server=<server> --docker-username=<user> --docker-password=<pass>

# View secret (base64 encoded)
kubectl get secret <name> -o yaml
```

## Resource Management

```bash
# Get all resources
kubectl get all
kubectl get all -n <namespace>

# Get specific resource
kubectl get <resource-type>
kubectl get pods,services,deployments

# Apply manifest
kubectl apply -f <file.yaml>
kubectl apply -f <directory>/

# Delete from manifest
kubectl delete -f <file.yaml>

# Edit resource
kubectl edit <resource-type> <name>
kubectl edit deployment <name>

# Get resource YAML
kubectl get <resource-type> <name> -o yaml
kubectl get pod <name> -o yaml

# Get resource JSON
kubectl get <resource-type> <name> -o json
```

## Debugging and Troubleshooting

```bash
# Get events
kubectl get events
kubectl get events --sort-by=.metadata.creationTimestamp

# Top nodes/pods
kubectl top nodes
kubectl top pods
kubectl top pods -n <namespace>

# Debug pod
kubectl debug <pod-name> -it --image=busybox

# Copy file from pod
kubectl cp <pod-name>:/path/to/file /local/path

# Copy file to pod
kubectl cp /local/path <pod-name>:/path/to/file
```

## Advanced Commands

```bash
# Watch resources
kubectl get pods -w
kubectl get pods --watch

# Label resources
kubectl label pod <name> env=production
kubectl label pod <name> env-  # Remove label

# Annotate resources
kubectl annotate pod <name> key=value

# Drain node
kubectl drain <node-name> --ignore-daemonsets

# Cordon/Uncordon node
kubectl cordon <node-name>
kubectl uncordon <node-name>

# Taint node
kubectl taint nodes <node-name> key=value:NoSchedule
kubectl taint nodes <node-name> key-  # Remove taint
```

## Useful Aliases

```bash
# Add to ~/.bashrc or ~/.zshrc
alias k='kubectl'
alias kgp='kubectl get pods'
alias kgs='kubectl get services'
alias kgd='kubectl get deployments'
alias kdp='kubectl describe pod'
alias kds='kubectl describe service'
alias kdd='kubectl describe deployment'
alias kl='kubectl logs'
alias kex='kubectl exec -it'
alias kaf='kubectl apply -f'
alias kdf='kubectl delete -f'
```

## Common YAML Templates

```yaml
# Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-app:latest
        ports:
        - containerPort: 8080

---
# Service
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app
  ports:
  - port: 80
    targetPort: 8080
  type: LoadBalancer
```
