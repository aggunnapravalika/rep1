# NGINX on AKS (k8s)

This folder contains manifests to run a simple NGINX deployment and an NGINX Ingress controller on Azure Kubernetes Service (AKS).

Files
- `nginx-azure-deployment.yaml`: Deployment, ConfigMap-backed index.html, and a `LoadBalancer` Service named `nginx-azure-lb` (in `default` namespace).
- `nginx-azure-ingress-controller.yaml`: Ingress-NGINX controller (namespace `ingress-nginx`) and an `Ingress` resource that routes to `nginx-azure-lb`.

Quick apply

1. Create the app resources:

```bash
kubectl apply -f k8s/nginx-azure-deployment.yaml
```

2. Install the ingress controller + Ingress (will provision an Azure public IP for the controller Service):

```bash
kubectl apply -f k8s/nginx-azure-ingress-controller.yaml
```

Notes
- The `Ingress` in `nginx-azure-ingress-controller.yaml` uses host `nginx.example.com` — replace it with your DNS name or remove the `host` entry to match any host.
- The app Service `nginx-azure-lb` is currently `LoadBalancer`. If you prefer the Ingress to be the external entrypoint, change `nginx-azure-lb` to `ClusterIP` and rely on the ingress controller Service instead.

Verify
- Check the app Service and external IPs:

```bash
kubectl get svc -n ingress-nginx
kubectl get svc nginx-azure-lb
kubectl get ingress -A
```

- Once the controller has an external IP, point DNS to it for your host and curl:

```bash
curl -H "Host: nginx.example.com" http://<INGRESS_EXTERNAL_IP>/
```

Contact me if you want the README expanded with CI steps, TLS (cert-manager), or split into separate files.

topics:
    helm