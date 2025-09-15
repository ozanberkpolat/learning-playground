HPA Autoscaling Demo notes:
- Ensure metrics-server is enabled: 
minikube addons enable metrics-server

- Install chart: 
helm install hpa-demo ./hpa-demo

- To watch HPA: 
kubectl get hpa -n hpa-demo --watch

- The load-generator pod will continuously create CPU load; delete it to stop load:
  kubectl delete pod load-generator -n hpa-demo
