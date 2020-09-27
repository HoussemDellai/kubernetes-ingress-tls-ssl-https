# kubernetes-ingress-tls-ssl-https
Demoing configuration for Ingress and HTTPS/TLS/SSL in Kubernetes. 

```bash

# Create a namespace for the ingress
kubectl create namespace ingress

# Add the Helm chart for Nginx Ingress
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update



```
