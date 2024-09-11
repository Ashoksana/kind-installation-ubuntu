# Setting up a Multi-Node Kind Cluster on Ubuntu 22.04

In this guide, we will walk through the process of setting up a multi-node Kind cluster on a Linux (Ubuntu 22.04) host.

## Prerequisites

- A machine running **Ubuntu 22.04**
- **Docker** installed on the host

## Install Kind

Execute the script below on the host to install the `kind` command. This script is based on the official Kind installation.

```bash

#!/bin/bash

# For AMD64 / x86_64
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
chmod +x ./kind
sudo cp ./kind /usr/local/bin/kind
rm -rf kind

After running the above script, the kind command will be available on your system.

To verify the installation, run:
```bash

kind --version

Expected output:
```bash

kind version 0.20.0
Bringing Up a Multi-Node Cluster
Create the Kind Cluster Configuration
Create a file named config.yml with the following content:
yaml
# 4 node (3 workers) cluster config
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  image: kindest/node:v1.28.0
- role: worker
  image: kindest/node:v1.28.0
- role: worker
  image: kindest/node:v1.28.0
- role: worker
  image: kindest/node:v1.28.0
Start the Cluster
Use the following command to start the multi-node Kind cluster using the config file:

bash

kind create cluster --config=config.yml
Expected output:
bash
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.28.0) 🖼
 ✓ Preparing nodes 📦 📦 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
 ✓ Joining worker nodes 🚜
Set kubectl context to "kind-kind"
After this, you can check the status of your cluster using the following commands.

Check Cluster Status
Using Kind:
kind get clusters
Expected output:

bash
kind

Using Kubectl:
To check the cluster info using kubectl:

bash

kubectl cluster-info
Expected output:
bash
Kubernetes control plane is running at https://127.0.0.1:41273
CoreDNS is running at https://127.0.0.1:41273/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

Using Docker:
Since the cluster nodes are Docker containers, you can list them with the following command:

bash
docker ps

Expected output:
bash
CONTAINER ID   IMAGE                  COMMAND                  CREATED      STATUS        PORTS                       NAMES
ed011f2dc7d7   kindest/node:v1.28.0   "/usr/local/bin/entr…"   4 days ago   Up 28 hours                               kind-worker
dc0940c24fce   kindest/node:v1.28.0   "/usr/local/bin/entr…"   4 days ago   Up 28 hours   127.0.0.1:41273->6443/tcp   kind-control-plane
acec2cacdc3b   kindest/node:v1.28.0   "/usr/local/bin/entr…"   4 days ago   Up 28 hours                               kind-worker2
8511cdf4bee7   kindest/node:v1.28.0   "/usr/local/bin/entr…"   4 days ago   Up 28 hours                               kind-worker3
Check Node Status

To check the status of each node in the cluster:

bash
kubectl get nodes

Expected output:

bash
NAME                 STATUS   ROLES           AGE   VERSION
kind-control-plane   Ready    control-plane   22m   v1.28.0
kind-worker          Ready    <none>          21m   v1.28.0
kind-worker2         Ready    <none>          21m   v1.28.0
kind-worker3         Ready    <none>          21m   v1.28.0
Verify Kubernetes Cluster Version
You can check the version of the cluster vs the version of the client (kubectl):

bash
kubectl version
Expected output:

bash
Client Version: v1.28.3
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
Server Version: v1.28.0
This indicates that the Kubernetes cluster is running version 1.28.0, while the client (kubectl) is at version 1.28.3.

Conclusion
You have successfully set up a multi-node Kind cluster on your Ubuntu 22.04 host. You can now interact with the cluster using kubectl commands or inspect the containers running the nodes with docker.

For more information and advanced usage, check out the official Kind documentation.
