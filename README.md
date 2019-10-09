# kubernetes-cluster-customization
This article shows you how to deploy the NGINX ingress controller in a Kubernetes cluster.

An ingress controller is a piece of software that provides a reverse proxy, configurable traffic routing, and TLS termination for Kubernetes services. Kubernetes ingress resources are used to configure the ingress rules and routes for individual Kubernetes services. Using an ingress controller and ingress rules, a single IP address can be used to route traffic to multiple services in a Kubernetes cluster.

# Namespace
First of all, create a  namespace where deploy your applications

```bash
kubectl create namespace <namespace name> --context=<k8s context name>
```
# Import SSL certificate
Import your own SSL certificate

```bash
kubectl create secret tls <certificate secret name> --namespace=<namespace name> --key ./certificate.key --cert ./certificate.crt --context=<k8s context name>
kubectl create secret tls <certificate secret name> --namespace=kube-system --key ./certificate.key --cert ./certificate.crt --context=<k8s context name>
```

# Deploy the Ingress Nginx Controller
Check to use the latest controller version, in nginx-ingress-controller-deployment.yml, and custom configuration to pass through a ConfigMap, nginx-custom-config.yml

```bash
kubectl apply -n kube-system -f ingress/serviceaccount.yml --context=<k8s context name>
kubectl apply -n kube-system -f ingress/rbac.yml --context=<k8s context name>
kubectl apply -n kube-system -f ingress/default-backend-deployment.yml --context=<k8s context name>
kubectl apply -n kube-system -f ingress/default-backend-service.yml --context=<k8s context name>
kubectl apply -n kube-system -f ingress/nginx-ingress-controller-deployment.yml --context=<k8s context name>
kubectl apply -n kube-system -f ingress/nginx-ingress-controller-service.yml --context=<k8s context name>
```

# Deploy your application

```bash
kubectl apply -n <namespace name> --context=<k8s context name> -f deployment-configurations/application.yml 
```

# Compatibility 
This configuration was tested on Azure Kubernetes Services with K8s version 1.13.10
