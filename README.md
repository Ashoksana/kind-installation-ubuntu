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
```
After running the above script, the kind command will be available on your system.

To verify the installation, run:
```bash
kind --version
```
Expected output:
```bash
kind version 0.20.0

