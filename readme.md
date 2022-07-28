## K8S Cluster 1 Master and 2 Workers using Ansible
Kubernetes is open-source and enterprise-ready container orchestration systems. Here we deploy Kubernetes(K8S) cluster with 1 master and 2 workers node and for this we used Ansible to automate the cluseter installation.

# Installation Steps
- Create User for Kubernetes on each node
- Install Kubernetes and containerd on each node
- Configure Master node
- Join the Worker nodes to the new K8s cluster

Before install we need to configure ansible server client and ansure host reachability.

## Install K8S cluster

Preparing Ansible script for Deploy Kubernetes cluster. Below is list of yaml files.
- [hosts] - For host list includes master and worker node info   
- [users.yml] - kube user configuration script
- [install-kubernetes.yaml] - Install master and worker nodes script.
- [cni-install.yaml] - For host list includes master and worker node info  
- [join-workers.yml] - Worknode join ansible script

Run below command for to install k8s user in worker and master node servers. Before install please update the host file with IP
```sh
$ ansible-playbook -i hosts users.yml
```

Install Kubernetes packages in 3 nodes.
```sh
$ ansible-playbook -i hosts install-kubernetes.yaml
```

Creating a Kubernetes CNI in Master node for cluster
```sh
$ ansible-playbook -i cni-install.yaml
```

Join worker nodes in K8s cluster
```sh
$ ansible-playbook -i hosts join-workers.yml
```

# Deploy the Metrics Server
Deploy the Metrics Server with the following command:
```sh
$ kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

# Deploying the Dashboard UI
The Dashboard UI is not deployed by default. We need deploy it, please run the following command:
```sh
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.5.0/aio/deploy/recommended.yaml
```

