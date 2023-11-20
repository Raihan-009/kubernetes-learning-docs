# Journey to NGINX Ingress Controller

## Lets create a k8s cluster using KinD
KinD (Kubernetes in Docker) is a tool for running local Kubernetes clusters using Docker container “nodes”. This is a light weight kubernetes cluster in docker containers.

```
kind create cluster --name nginx-ingress --image kindest/node:v1.23.5
```

Oppss! Seems i got some issues. I got an error.
```
zsh: command not found: kind
```

Since I am using mac and docker-desktop i need to install kind separately.

```
brew install kind
```

## Run a workable container

Its like a virtual environment where we can install our dependencies

### Run Apline Linux container

```
docker run -it --rm -v ${HOME}:/root/ -v ${PWD}:/workspace -w /workspace --net host alpine sh
```

### Install dependencies

- Install curl
    ```
    apk add --no-cache curl
    ```
- Install kubectl
    ```
    curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
    chmod +x ./kubectl
    mv ./kubectl /usr/local/bin/kubectl
    ```
- Install helm
    ```
    curl -o /tmp/helm.tar.gz -LO https://get.helm.sh/helm-v3.10.1-linux-amd64.tar.gz
    tar -C /tmp/ -zxvf /tmp/helm.tar.gz
    mv /tmp/linux-amd64/helm /usr/local/bin/helm
    chmod +x /usr/local/bin/helm
    ```c

Here we go! All environmental setup is now ready. We can perform our k8s operations.

# NGINX Controller

## Getting Installation yml
To add ingress-nginx repo using helm.
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```
To check all the versions.
```
helm search repo ingress-nginx --versions
```

```
NAME                       	CHART VERSION	APP VERSION	DESCRIPTION
ingress-nginx/ingress-nginx	4.8.3        	1.9.4      	Ingress controller for Kubernetes using NGINX a...
```

Set environment variable
```
CHART_VERSION="4.8.3"
APP_VERSION="1.9.4"
```

Now, make one more directory and get the manifest yml.
```
mkdir ./ingress/controller/nginx/manifests/

helm template ingress-nginx ingress-nginx \
--repo https://kubernetes.github.io/ingress-nginx \
--version ${CHART_VERSION} \
--namespace ingress-nginx \
> ./ingress/controller/nginx/manifests/nginx-ingress.${APP_VERSION}.yaml
```

## Now Lets deploy the ingress controller

Create namespace
```
kubectl create namespace ingress-nginx
```
Go to the respective yml file directory and deploy

```
kubectl apply -f nginx-ingress.${APP_VERSION}.yml
```

## Check installation
```
kubectl -n ingress-nginx get pods
```

```
kubectl -n ingress-nginx get svc
``````
```
/workspace/ingress/controller/nginx/manifests # kubectl -n ingress-nginx get svc
NAME                                 TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)                      AGE
ingress-nginx-controller             LoadBalancer   10.96.170.2    <pending>     80:30592/TCP,443:31536/TCP   3m47s
ingress-nginx-controller-admission   ClusterIP      10.96.66.193   <none>        443/TCP                      3m47s
```

The traffic for our cluster will come in over the Ingress service
Note that we dont have load balancer capability in kind by default, so our LoadBalancer is pending.

## Setup port-forwarding
```
kubectl -n ingress-nginx port-forward svc/ingress-nginx-controller 443
```
```
/workspace/ingress/controller/nginx/manifests # kubectl -n ingress-nginx port-forward svc/ingress-nginx-controller 443
Forwarding from 127.0.0.1:443 -> 443
Forwarding from [::1]:443 -> 443

```