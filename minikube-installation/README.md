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


## Then what does minikube do?