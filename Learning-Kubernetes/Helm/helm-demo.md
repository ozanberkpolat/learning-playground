# Monitoring NGINX Pods with Prometheus & Grafana on Minikube

This guide shows how to deploy Prometheus, Grafana, and 5 NGINX pods (via a ReplicaSet) in Minikube, generate load, and then visualize CPU and memory utilization for the NGINX pods using Grafana.

---

## Step 1: Clean Up Old Resources

Delete any old Prometheus, Grafana, or other resources:

```powershell
helm uninstall monitoring
kubectl delete all --all -n nginx-demo
kubectl delete namespace nginx-demo
```

---

## Step 2: Deploy Prometheus + Grafana

We use the **kube-prometheus-stack** Helm chart, which includes Prometheus, Grafana, and exporters.

```powershell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install monitoring prometheus-community/kube-prometheus-stack
```

---

## Step 3: Create Namespace for NGINX

```powershell
kubectl create namespace nginx-demo
```

---

## Step 4: Deploy 5 NGINX Pods via a ReplicaSet

Create a file named `nginx-replicaset.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: nginx-demo
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
---
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
  namespace: nginx-demo
spec:
  replicas: 5
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:stable
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: "100m"
            memory: "128Mi"
          limits:
            cpu: "250m"
            memory: "256Mi"
```

Apply it:

```powershell
kubectl apply -f nginx-replicaset.yaml
```

Check that all 5 pods are running:

```powershell
kubectl get pods -n nginx-demo
```

---

## Step 5: Deploy a Load Generator Pod

Create a file named `load-generator.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: load-generator
  namespace: nginx-demo
spec:
  containers:
  - name: busybox
    image: busybox
    command: ["/bin/sh"]
    args:
      - "-c"
      - "while true; do wget -qO- -T 1 -O /dev/null http://nginx-service; done"
```

Apply it:

```powershell
kubectl apply -f load-generator.yaml
```

Check logs to see activity:

```powershell
kubectl logs -f load-generator -n nginx-demo
```

---

## Step 6: Access Grafana

Expose Grafana with:

```powershell
minikube service monitoring-grafana
```

* **Username:** `admin`
* **Password:** `prom-operator` (default, unless changed)

---

## Step 7: Monitor NGINX Namespace

1. Open Grafana in your browser.
2. Go to **Dashboards → Manage**.
3. Open the **Kubernetes / Compute Resources / Namespace (Pods)** dashboard.
4. Select **Namespace: `nginx-demo`** from the dropdown.

✅ You’ll now see CPU and memory usage aggregated for all 5 NGINX pods in the ReplicaSet.

---

### Notes

* You can scale the ReplicaSet manually:

```powershell
kubectl scale rs nginx-replicaset --replicas=10 -n nginx-demo
```

* The load generator continuously sends requests to NGINX, so you can see the metrics updating in Grafana.
* This setup keeps NGINX pods isolated in their own namespace for easier monitoring.
