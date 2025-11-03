# Devops Playground


## Local Kubernetes Cluster 

### Docker Setup
Force Docker to use systemd cgroup driver (ignore if already working)

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"]
}
EOF
sudo systemctl restart docker
```

### Create cluster
This will create a 3 node on device Kubernetes cluster with a control pane that runs on 0.0.0.0:8080

kind create cluster --config kind-cluster/config.yaml

### Delete cluster
kind delete cluster


### Get cluster info
kubectl cluster-info --context kind-kind

### Confirm cluster with kubectl 
kubectl get no


# ArgoCD

Install initial ArgoCD

```bash
kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


kubectl wait --namespace argocd \
  --for=condition=available deployment/argocd-server \
  --timeout=300s


kubectl get pods -n argocd

```

# One‑time bootstrap (run on your already‑running cluster)

## Install ArgoCD
```bash
kubectl apply -f argocd/bootstrap.yaml
```

# Wait for ArgoCD to be ready
```bash
kubectl wait -n argocd --for=condition=available deployment/argocd-server --timeout=5m
```




-------------------------------------
# Get ArgoCD Initial password

```bash
argocd admin initial-password -n argocd | head -1
```

# Port‑forward UI (optional, for login)
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443 &
```

# ArgoCD CLI login
argocd login localhost:8080 --username admin --password <your_admin_password> --insecure

# Configure Git Repository 
argocd repo add https://github.com/<YOUR_USER>/<YOUR_REPO>.git --username <USER> --password <TOKEN>

# Bootstrap ArgoCD
This will use the git repository we just added

```bash
kubectl apply -f argocd/bootstrap.yaml
```

# Check ArgoCD logs
```bash
kubectl logs -n argocd \
  -l app.kubernetes.io/name=argocd-application-controller \
  --tail=50 | grep -i demo
```


--------------------------------

## Open web browser
https://localhost:8080



# Using

Open http://localhost:8080
User: admin
Password:

argocd admin initial-password -n argocd | head -1





kubectl apply -f argocd/apps/traefik/application.yaml
kubectl apply -f argocd/apps/demo/application.yaml



watch kubectl get applications -n argocd


echo "127.0.0.1 demo.local" | sudo tee -a /etc/hosts
curl -H "Host: demo.local" http://localhost
# → <title>Welcome to nginx!</title>

