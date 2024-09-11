# kind-installation-ubuntu

Install Kind
Execute the below script on the host to install `kind` command on the host. This script has been inspired from Kind installation

#!/bin/bash

# For AMD64 / x86_64
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
chmod +x ./kind
sudo cp ./kind /usr/local/bin/kind
rm -rf kind
After the above script is executed on the host, kind command will be accessible on the host.

[binita@test-kubernetes]# kind --version
kind version 0.20.0
[binita@test-kubernetes]#
Bring up a multi node cluster
Create a config file (`config.yml`) with the below content:

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
Start the 4 node cluster on this host:

[binita@test-kubernetes]# kind create cluster --config=config.yml
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.28.0) 🖼
 ✓ Preparing nodes 📦 📦 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
 ✓ Joining worker nodes 🚜
Set kubectl context to "kind-kind"
You can now use your cluster with:

kubectl cluster-info --context kind-kind

Not sure what to do next? 😅  Check out https://kind.sigs.k8s.io/docs/user/quick-start/
[binita@test-kubernetes]#
Check the cluster status:

Using kind based command:

[binita@test-kubernetes]# kind get clusters
kind
Using kubectl based command. If you do not have kubectl installed, please refer to install kubectl linux to install it:

[binita@test-kubernetes]# kubectl cluster-info
Kubernetes control plane is running at https://127.0.0.1:41273
CoreDNS is running at https://127.0.0.1:41273/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Using docker based command. It is worthwhile to note that all the nodes (control-plane and worker) comprising your cluster will be running as docker containers on your host. This can be easily deduced from the output of the below command:

[binita@test-kubernetes]# docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED      STATUS        PORTS                       NAMES
ed011f2dc7d7   kindest/node:v1.28.0   "/usr/local/bin/entr…"   4 days ago   Up 28 hours                               kind-worker
dc0940c24fce   kindest/node:v1.28.0   "/usr/local/bin/entr…"   4 days ago   Up 28 hours   127.0.0.1:41273->6443/tcp   kind-control-plane
acec2cacdc3b   kindest/node:v1.28.0   "/usr/local/bin/entr…"   4 days ago   Up 28 hours                               kind-worker2
8511cdf4bee7   kindest/node:v1.28.0   "/usr/local/bin/entr…"   4 days ago   Up 28 hours                               kind-worker3
Check the status of each node on the cluster:

[binita@test-kubernetes]# kubectl get nodes
NAME                 STATUS   ROLES           AGE   VERSION
kind-control-plane   Ready    control-plane   22m   v1.28.0
kind-worker          Ready    <none>          21m   v1.28.0
kind-worker2         Ready    <none>          21m   v1.28.0
kind-worker3         Ready    <none>          21m   v1.28.0
[binita@test-kubernetes]#
Check the version of the cluster vs the version of the cluster client (kubectl)

[binita@test-kubernetes]# kubectl version
Client Version: v1.28.3
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
Server Version: v1.28.0
From the output of the above command, we can see that the cluster is running on version 1.28.0, whereas the client is on 1.28.3.

Takeaways
We saw that kind runs each Kubernetes node as a docker container within our host. A host could be a physical machine/VM.
It is important to emphasize that kind is not designed to support production grade Kubernetes cluster.
A production grade cluster would have the different nodes spread across multiple host.
However, setting up a production grade cluster is out of kind’s scope. It is only meant for local testing. This topic has been extensively discussed at multi machine kind cluster
