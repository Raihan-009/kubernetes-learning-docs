# Minikube installation process

## Lets have a look to the kubernetes cluster architecture

![k8s arch](https://github.com/Raihan-009/kubernetes-developments/blob/main/minikube-installation/architecture/multi-node-cluster.png)

### Here, lets undestand what does a <span style="color: green;">**Node**</span> means?

<span style="color: green;">**Node**</span> refers a machine (physical or virtual), also known as **Minions**, where kubernetes can be installed, or worker containers will launched by k8s.

### what does a <span style="color: green;">**Cluster**</span> means?

A <span style="color: green;">**Cluster**</span> is  a set of nodes grouped together. Basically <span style="color: green;">**Cluster**</span> deals with more than one node and it is possible to be with only one node too.

### Master node and Worker node

<span style="color: #f59842;">**Master**</span> is a node with k8s installed in it and is configured as a master to watch over all the nodes in the current cluster and responsible of the actual orchestration.

<span style="color: #f59842;">**Worker Node**</span> refers again like a machine, physical or virtual where container will be launched by master node (k8s).

![worker-master](https://github.com/Raihan-009/kubernetes-developments/blob/main/minikube-installation/architecture/master-worker-node.png)


## Then, what does Minikube do??

Minikube is one node cluster where master process and worker processes both run on one physical or virtual machine, including pre-install Docker.

The minikube architecture -->

![minikube arch](https://github.com/Raihan-009/kubernetes-developments/blob/main/minikube-installation/architecture/minikube-architecture.png)

## Let's dive into the installation process.

For official docs you can [click here](https://minikube.sigs.k8s.io/docs/start/).

To get host's architecture, run
```
uname -m
```
To install the latest minikube stable release on x86-64 Linux using binary download:
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
Now, in order to start cluster, from a terminal with administrator access (but not logged in as root), run:
```
minikube start
```
Let's see now...
![minikube failed](https://github.com/Raihan-009/kubernetes-developments/blob/main/minikube-installation/examples/minikube-failed.png)

Oppss!!... It seems minikube fails to start. It's likely a problem with the underlying virtualization or container runtime that Minikube is using. So we need to explicitly specify the driver when starting Minikube to ensure that it uses the correct one.

Run
```
minikube start --driver=docker
```
The Docker driver allows you to install Kubernetes into an existing Docker install. For more [click here](https://minikube.sigs.k8s.io/docs/drivers/).

Lets see what is going on now...
![permission denied](https://github.com/Raihan-009/kubernetes-developments/blob/main/minikube-installation/examples/requires-driver.png)

Ummmmm.... It's likely related to the user's permissions to access the Docker daemon socket. By default, Docker requires elevated privileges, and using sudo is a common approach. However, we can also configure Docker to allow our user account to access the Docker daemon without using sudo.

Run
```
sudo usermod -aG docker $USER and newgrp docker
minikube start --driver=docker
```

![start](https://github.com/Raihan-009/kubernetes-developments/blob/main/minikube-installation/examples/usermod-and-minikube-start.png)

Yes! Now everything is good, this will take few minutes to start the cluster and we can check the status running
```
minikube status
```

![minikube status](https://github.com/Raihan-009/kubernetes-developments/blob/main/minikube-installation/examples/minikube-status.png)

To stop minikube, run
```
minikube stop
minikube status
```

![minikube stop](https://github.com/Raihan-009/kubernetes-developments/blob/main/minikube-installation/examples/minikube-stop-and-status.png)

