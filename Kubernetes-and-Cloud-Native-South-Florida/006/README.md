# Title

Episode 006: SSL + Let's Encrypt in Kubernetes with Cert-manager

## Table of contents

## Requirements

We are using a simple cluster in **Digital Ocean (this is not a sponsor we chose this cloud provider for simplicity)**, to follow along this example just run the following comands. Be aware you must have an account in Digital Ocean.

### Setup Digital Ocean

```shell
# To login in your Digital Ocean account if you haven't and follow the steps in your terminal.
doctl auth init

# To create a simple cluster you can change the name of your cluster.
doctl kubernetes cluster create sflo-ep006

# Once the cluster is created the kubectl tool will be configured to use the new cluster you can check this by doing.
kubectl config current-context
```

### Setup Nginx ingress controller

Although you can use any ingress controller in this example will use Nginx ingress controller, is just simple to setup and most people know Nginx; however it does not play any special role other than create a service with a public IP that we can use for our external access.

```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.44.0/deploy/static/provider/do/deploy.yaml
```

```shell
# Using helm let's add ingres-nginx to our list of repos and update.
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

# setup nginx for digital ocean there are a couple annotations we need.
helm install ingress-nginx ingress-nginx/ingress-nginx \
    --set controller.service.annotations="service.beta.kubernetes.io/do-loadbalancer-enable-proxy-protocol=true"
```


## Install

```shell
kubectl create namespace cert-manager

helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager jetstack/cert-manager --namespace cert-manager --version v1.1.0 --set installCRDs=true

kubectl get pods --namespace cert-manager
```
