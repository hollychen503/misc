# Kubernetes in China

Thanks to Aliyun mirrors for both OS packages and docker images, in spring/summer 2018 this got a lot easier.

This guide uses kubeadm to set up Kubernetes in China, either as a cluster in Aliyun or on your local machine. Details:

- Ubuntu 16.04
- Single master (non-HA). Should be straightforward to adapt to HA though.
- Integration with VPC networking and storage

Not covered:

- Backup and Upgrading (maybe coming soon?)
- Tool-assisted machine provisioning (you'll need to do it manually)
- Dynamic disk provisioning or load balancer integration. There are drivers for this, I just haven't investigated them.


## Aliyun Cluster (master)

To setup a new master, follow these steps:

1. [Setup machines](setup-machines.md)
2. [Install Docker](install-docker.md)
3. [Pick a Kubernetes version](pick-k8s-version.md)
4. [Install kubelet and kubeadm](install-kubelet-kubeadm.md)
5. [Configure kubelet and kubeadm for use with a cluster](customize-cluster.md)
6. [Run `kubeadm init`](kubeadm-init.md)
7. [Networking](networking.md)


## Aliyun Cluster (workers)

To setup a new worker, you'll still need to perform a subset of the instructions to setup a new master:

1. [Setup machines](setup-machines.md)
2. [Install Docker](install-docker.md)
3. [Install kubelet and kubeadm](install-kubelet-kubeadm.md)
4. [Join an existing cluster](join-existing-cluster.md)


## Minikube replacement

Minikube is a self-contained, well-supported onebox kubernetes installation.  It's finicky to install in China, and not all versions of kubernetes have a corresponding minikube version.

To use kubeadm instead of minikube, follow these steps:

1. [Install Docker](install-docker.md)
2. [Install kubelet and kubeadm](install-kubelet-kubeadm.md)


## Backup and Upgrades

\# TODO

There are two gaping holes in this guide:

- This is a single-master Kubernetes cluster. That's not _terrible_... a master going down just means that new workloads can't be scheduled, not that existing ones break. This might be the right tradeoff for you. However, if you lose the master's disk, etcd configuration goes with it, and the cluster must be rebuilt from scratch. It must be periodically backed up.
- Upgrading isn't addressed at all. This is something that kops does extremely well. Doing it manually on a cluster of modest size shouldn't be terrible -- drain a node, upgrade its components, re-add it to the cluster -- but there's a lot of potential for things to go wrong.