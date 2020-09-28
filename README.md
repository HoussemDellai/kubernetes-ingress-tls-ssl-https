# kubernetes-ingress-tls-ssl-https
Demoing configuration for Ingress and HTTPS/TLS/SSL in Kubernetes. 

```bash

# Create a namespace for the apps
kubectl apply -f app-namespace.yaml

# Deploy the 2 sample apps into Kubernetes
kubectl apply -f app1-deploy-svc.yaml 
kubectl apply -f app2-deploy-svc.yaml

# Get the 2 public IP addresses 
kubectl get services --namespace app

# Add the Helm chart for Nginx Ingress
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

# Install the Helm (v3) chart for nginx ingress controller
helm install app-ingress ingress-nginx/ingress-nginx --namespace ingress --create-namespace --set controller.replicaCount=2 --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux

# Get the Ingress Controller public IP address
kubectl get services --namespace ingress

# Update the service type to ClusterIP instead of LoadBalancer 
# in app-deploy.yaml file
# Delete and redeploy the service for the update to take effect
kubectl delete -f app1-deploy-svc.yaml 
kubectl delete -f app2-deploy-svc.yaml
kubectl apply -f app1-deploy-svc.yaml 
kubectl apply -f app2-deploy-svc.yaml

# Deploy the Ingress resource into Kubernetes
kubectl apply -f app-ingress.yaml 

# Cleanup resources
kubectl delete -f app1-deploy-svc.yaml 
kubectl delete -f app2-deploy-svc.yaml
kubectl delete -f app-namespace.yaml
helm delete app-ingress --namespace ingress
kubectl delete namespace ingress

```

IMPORTANT NOTE: The Ingress and the Services should be inside the same Namespace. Otherwise, the Ingress won't find the Service even with its full name: <service-name>.<namespace>.svc.local
