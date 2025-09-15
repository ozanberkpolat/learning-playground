# Learning Cloud & Container Orchestration

This repository contains hands-on experiments and configurations for learning **Docker Compose**, **Kubernetes**, and **Helm**. It serves as a personal playground to practice containerization, orchestration, and deployment strategies.

---

## 🐣 Docker Compose

### Start Services

```bash
docker-compose up -d
```

### Stop Services

```bash
docker-compose down
```

### Inspect Logs

```bash
docker-compose logs -f
```

This setup is useful for testing multi-container applications locally.

---

## ☨️ Kubernetes

### Apply Manifests

```bash
kubectl apply -f k8s/
```

### Delete Resources

```bash
kubectl delete -f k8s/
```

### Check Resources

```bash
kubectl get pods,svc,deployments -n <namespace>
```

### Namespace Management

```bash
kubectl create namespace my-namespace
kubectl config set-context --current --namespace=my-namespace
```

---

## 📦 Helm Charts

### Install a Chart

```bash
helm install my-release helm/my-chart
```

### Upgrade a Chart

```bash
helm upgrade my-release helm/my-chart
```

### Uninstall a Chart

```bash
helm uninstall my-release
```

Helm charts in this repo are used to practice templating, values management, and deploying complex applications to Kubernetes.

---

## 🔧 Tools & Requirements

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/)
- [Kubernetes](https://kubernetes.io/docs/setup/)
- [Helm](https://helm.sh/docs/)

---

## 📚 Learning Goals

- Understand container orchestration concepts.
- Practice deploying applications locally and in Kubernetes clusters.
- Explore Helm templating and chart management.
- Compare local Docker Compose setups with Kubernetes deployments.

---

## 📚 References

- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Helm Documentation](https://helm.sh/docs/)

