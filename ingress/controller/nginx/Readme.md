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