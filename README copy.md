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

## Install ArgoCD CLI
curl -sSL -o argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
chmod +x argocd && sudo mv argocd /usr/local/bin/