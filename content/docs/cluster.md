+++
title = "Creating the cluster"
weight = "2"
+++

The first step is to create a Kubernetes cluster in your DigitalOcean account. You can do this via the user interface:

![creating the Kubernetes cluster](/cf4k8s-do/img/do-1.png "Creating a Kubernetes Cluster on DigitalOcean")

Wait for the cluster to be ready.

![waiting for the cluster](/cf4k8s-do/img/do-2.png "Wait for the cluster to be ready")

Alternatively, here's the doctl CLI command for those who prefer to create clusters without leaving the comfort of their console:

```
doctl kubernetes cluster create cf4k8s-demo --node-pool "name=cf-k8s;size=s-4vcpu-8gb;count=5;tag=cf-k8s-ftw" --version 1.17.11-do.1 --update-kubeconfig true --set-current-context true --region sfo2 

```

Each of the flags used are explained below: 

`-- node-pool` the list of node pools, defined using semi-colon separated configuration values. In this case, we use the following-
* `name` A unique name for the node-pool
* `size` Machine size slug to be used
* `count` Number of nodes
* `tag` Tags to identify node pool

`--version` Kubernetes version running on the nodes

`--update-kubeconfig` Boolean that specifies adding a new configuration context to kubectl 

`--set-current-context` Boolean that specifies whether to set the kubectl context to point to the new cluster 

`--region` Specifies which region the Kubernetes cluster should be created in. 

Note: By default, a cluster with 3-nodes and 1 node-pool will spin up and located in the nyc1 region, using the latest available version of Kubernetes.
