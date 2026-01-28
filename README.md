# EC2 - prometheus-monitoring
## 
```
https://dineshjadhav.hashnode.dev/monitoring-using-prometheus-and-grafana-on-aws-ec2
```
```
https://dev.to/aws-builders/monitor-your-static-app-memory-usage-ec2-instances-with-prometheus-and-grafana-21ln
```

# EKS- prometheus-monitoring

## Helm
```
curl -sSL https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
helm version --short
```
## Prometheus - Grafana
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```
```
kubectl create namespace monitoring
helm install prometheus prometheus-community/kube-prometheus-stack --namespace monitoring
```
```
kubectl get pods -n monitoring
```
```
kubectl get svc -n monitoring
```
```
kubectl patch svc prometheus-kube-prometheus-prometheus -n monitoring -p '{"spec": {"type": "LoadBalancer"}}'
kubectl patch svc prometheus-grafana -n monitoring -p '{"spec": {"type": "LoadBalancer"}}'
```
```
kubectl get secrets -n monitoring
```
```
# Sustituye 'NOMBRE_DEL_SECRET' por el que encontraste
kubectl get secret -n monitoring NOMBRE_DEL_SECRET -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
```
Grafana comes with pre-configured dashboards for Kubernetes monitoring. Once you're logged into Grafana, follow these steps to set them up:

Prometheus Data Source: If you setup Prometheus and Grafana using the Kube-Prometheus stack, this data source will already be configured. If you have installed Prometheus and Grafana separately, you will need to setup Prometheus as a data source.
Navigate to Connections -> Data Sources.
Add a new data source and choose "Prometheus."
Enter the Prometheus service URL: http://prometheus-kube-prometheus-prometheus.monitoring.svc:9090.
Click "Save & Test" to ensure Grafana can connect to Prometheus.
Import Pre-Built Dashboards: Grafana allows you to import community-built dashboards specifically designed for Kubernetes monitoring.
Go to Dashboards -> New -> Import.
Use dashboard IDs from Grafana's official dashboard page (https://grafana.com/grafana/dashboards).
Copy the ID for any popular dashboard you want to use. For this post, we will use the "Node Exporter Full" dashboard.
```
```
ID: Node Exporter Full   1860
ID: K8s Dasboard         15661
```

# Links
```
https://mantalus.com/blog/prometheus-and-grafana-on-amazon-eks-2/
https://grafana.com/grafana/dashboards/
```

# Helm Apps
```
helm list -A
helm get values prometheus-kube-prometheus -n monitoring
```
## Nginx
```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```
```
helm install mi-web-demo bitnami/nginx \
  --set replicaCount=2 \
  --set service.type=LoadBalancer
```
```
kubectl get svc mi-web-demo-nginx
```
```
helm upgrade mi-web-demo bitnami/nginx \
  --set replicaCount=5 \
  --set service.type=LoadBalancer
```
```
helm uninstall mi-web-demo
```
```
helm pull bitnami/nginx --untar
cd nginx
ls -R
tree
```
## Wordpess
```
helm install wordpress-efimero bitnami/wordpress \
  --set service.type=LoadBalancer \
  --set persistence.enabled=false \
  --set mariadb.primary.persistence.enabled=false \
  --set wordpressUsername=admin \
  --set wordpressPassword=password123
```
```
kubectl get pvc
```
```
helm uninstall wordpress-efimero
```
```
helm list
```
