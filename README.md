# ChatApp Deployment Guide

## Docker Authentication

```bash
docker login -u rabbise
```
> Authenticate with generated token

---

## Build & Push Docker Images

### Backend
```bash
docker build -t rabbise/chatapp-backend:latest .
docker push rabbise/chatapp-backend:latest
```

### Frontend
```bash
docker build -t rabbise/chatapp-frontend:latest .
docker push rabbise/chatapp-frontend:latest
```

---

## Kubernetes Setup

### Create Namespace
```bash
kubectl create -f namespace.yml
```

### Storage
```bash
kubectl apply -f mongodb-pv.yml
kubectl apply -f mongodb-pvc.yml
```

### Services
```bash
kubectl apply -f mongodb-service.yml
kubectl apply -f backend-service.yml
kubectl apply -f frontend-service.yml
```

### Config & Secrets
```bash
kubectl apply -f configMap.yml
kubectl apply -f secret.yml
```

### Deployments
```bash
kubectl apply -f mongodb-deployment.yml
kubectl apply -f backend-deployment.yml
kubectl apply -f frontend-deployment.yml
```

### Ingress
```bash
kubectl apply -f ingress.yml
```

---

## Port Forwarding

### Direct Services
```bash
sudo -E kubectl port-forward --address 0.0.0.0 service/backend --namespace=chat-app 5001:5001 &
sudo -E kubectl port-forward --address 0.0.0.0 service/frontend --namespace=chat-app 80:80 &
```

### With Ingress
```bash
sudo -E kubectl port-forward --address 0.0.0.0 service/ingress-nginx-controller --namespace=chat-app 80:80 &
```

> **Note:** `minikube addons enable ingress` needs to run before port forwarding
