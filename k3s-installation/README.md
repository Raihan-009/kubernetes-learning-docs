# K3S installation process with K3D

## What is K3s?
k3s is a lightweight and efficient Kubernetes distribution designed for resource-constrained environments.

## Features of k3s
- Lightweight
- Single Binary
- Reduced Dependencies

## Single-server setup

![single server setup](https://github.com/Raihan-009/kubernetes-developments/blob/main/k3s-installation/architecture/single-server-setup.png)

## Why k3d?
k3d is a convenient tool for deploying and managing k3s clusters within Docker containers. K3d makes it very easy to create single and multi-node k3s clusters in docker, e.g. for local development on Kubernetes.

### Make sure you have docker installed and kubectl utility

To install Docker on a system using the apt package manager, run
```bash
sudo apt update
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
```

Install kubectl
```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
mv ./kubectl /usr/local/bin/kubectl
```

To check version of docker, run
```bash
docker --version
```

![version](https://github.com/Raihan-009/kubernetes-developments/blob/main/k3s-installation/examples/versions.png)


### K3d installation
Install current latest release using `curl`
```bash
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```

![installing k3d](https://github.com/Raihan-009/kubernetes-developments/blob/main/k3s-installation/examples/k3d-installation.png)

## Creating the K3s cluster
```bash
k3d cluster create matrix --api-port 6443 --servers 1 --agents 2 --port "30500-31000:30500-31000@server:0"
```

Here, we are creating a k3d cluster named "matrix" with the following specifications:

- --api-port 6443: Specifies the API server port for the Kubernetes cluster.

- --servers 1: Indicates the number of server nodes in the cluster. In this case, it's set to 1, meaning there is one server node.

- --agents 2: Specifies the number of agent nodes (or worker nodes) in the cluster. In this case, it's set to 2, meaning there are two agent nodes.

- --port "30500-31000:30500-31000@server:0": Exposes the port range 30500-31000 on the host and forwards it to the same port range on the first server node (server:0).

![cluster creating](https://github.com/Raihan-009/kubernetes-developments/blob/main/k3s-installation/examples/cluster-created.png)

To check cluster list, run
```bash
k3d cluster list
```
![cluster list](https://github.com/Raihan-009/kubernetes-developments/blob/main/k3s-installation/examples/cluster-list.png)

We can check the cluster-info, run
```bash
kubectl cluster-info
```

![cluster-info](https://github.com/Raihan-009/kubernetes-developments/blob/main/k3s-installation/examples/cluster-info.png)

To check all the nodes, run
```bash
kubectl get nodes
```
![cluster-info](https://github.com/Raihan-009/kubernetes-developments/blob/main/k3s-installation/examples/nodes.png)