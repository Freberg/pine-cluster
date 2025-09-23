# Pine Cluster

## Installation

### Argo

Install argocd
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Sealed secrets

Add sealed-secrets as an argo application
```
kubectl apply -f argo-applications/sealed-sercrets.yaml
```

### Kubernetes dashboard

Add kubernetes-dashboard as an argo application
```
kubectl apply -f argo-applications/kubernetes-dashboard.yaml
```

### Longhorn

Due to the space constrains of this mini cluster, not all nodes are created equal

Label storage nodes
```
kubectl label nodes <STORAGE_NODE> node.longhorn.io/create-default-disk=true
kubectl apply -f argo-applications/longhorn.yaml
```

## Good to know

Retrieve initial admin password for argocd
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Generate token for the kubernetes-dashboard
```
kubectl create token admin-user -n kubernetes-dashboard
```

k3s support single node deployments and does not taint the master node, to prevent pods being scheduled on master
```
kubectl taint nodes pine1 node-role.kubernetes.io/master:NoSchedule
```
