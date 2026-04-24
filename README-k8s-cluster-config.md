# DigitalOcean Kubernetes Cluster Setup Guide

## Overview

This guide provides step-by-step instructions for setting up a Kubernetes cluster on DigitalOcean, including the necessary tools and ingress controller configuration.

## Prerequisites

- DigitalOcean account with API access
- SSH client for VM access

## Setup Instructions

### Step 1: Obtain DigitalOcean Access Token

> **Important**: This token is sensitive - keep it secure and never commit it to version control.

1. Log in to your DigitalOcean account
2. Navigate to the API section
3. Generate an access token (format: `dop_v1_XXXX`)

### Step 2: Create Kubernetes Cluster

Create a new Kubernetes cluster in your DigitalOcean dashboard.

### Step 3: Set Up Controller Droplet/VM

1. Create a new Droplet/VM for your controller
2. Connect to the VM via SSH:

```bash
ssh user@ip-address
```

3. Install doctl (DigitalOcean CLI):

```bash
snap install doctl
mkdir /root/.config
doctl auth init
```

When prompted, enter your DigitalOcean access token from Step 1.

> **Tip**: The controller VM is used to manage your Kubernetes clusters remotely via doctl and kubectl.

4. Create the kube config directory:

```bash
mkdir /root/.kube
```

### Step 4: Configure Cluster Access

```bash
# Connect doctl to kube config
snap connect doctl:kube-config

# Save your cluster's kubeconfig
doctl kubernetes cluster kubeconfig save <KUBERNETES_CLUSTER_ID>

# View the config file
doctl kubernetes cluster kubeconfig show <KUBERNETES_CLUSTER_ID>
```

**Important:** Copy the config output for later use in your production and deployment secrets. This kubeconfig contains authentication information for cluster access.

### Step 5: Install kubectl

```bash
# Download kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

# Make executable and move to PATH
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

# Verify installation
kubectl version --client
```

> **Note**: This installs the latest stable version of kubectl that matches your cluster version.

Check cluster connectivity:

```bash
kubectl get ingress
```

## Step 6: Set Up NGINX Ingress Controller

### Why Ingress is Needed

Ingress controllers provide external access to services running inside the Kubernetes cluster, enabling features like:
- HTTP/HTTPS routing
- Load balancing
- SSL/TLS termination
- Virtual hosting
- Path-based routing

### Installation

Deploy the NGINX Ingress Controller:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

> **Warning**: Always verify the URL before applying YAML from remote sources in production environments.

### Verification

```bash
# Check general service status
kubectl get svc -n ingress-nginx

# Check ingress controller service specifically
kubectl -n ingress-nginx get svc ingress-nginx-controller

# Get the ingress IP address
kubectl -n ingress-nginx get svc ingress-nginx-controller -o jsonpath="{.status.loadBalancer.ingress[0].ip}"
```

### Accessing Your Application

After deploying your application with ingress resources, check the ingress status:

```bash
kubectl get ingress -n <your-namespace>
```

Example output:
```
NAME           CLASS    HOSTS                                            ADDRESS          PORTS     AGE
solar-system   <none>   solar-system-development.<IPv4>.nip.io   <IPv4>   80,443   6m18s
```

The nip.io service provides wildcard DNS, allowing access via the LoadBalancer IP with a subdomain format.

## Next Steps

1. Insert the copied configuration text mentioned in Step 04 as secrets inside both  `development` environment and `production` environment. 
2. Github Actions uses the environment secrets for Production and Deployment purpose inside Kubernetes Cluster.

## Troubleshooting

- Ensure your DigitalOcean access token has the correct permissions
- Verify cluster connectivity with `kubectl cluster-info`
- Check ingress controller logs: `kubectl logs -n ingress-nginx deployment/ingress-nginx-controller`
- Use `kubectl describe` commands to get detailed resource information
