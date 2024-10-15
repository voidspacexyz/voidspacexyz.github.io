---
title: "Kubernetes - Getting Started on Azure"
date: 2024-07-22
draft: false
garden_tags: ["kubernetes", "devops"]
summary: " "
status: "seeding"
---

# Kubernetes - Getting Started on Azure

Welcome to the exciting world of Kubernetes! In this hands-on guide, we'll walk you through setting up and managing your first Kubernetes cluster on Azure. By the end of this article, you'll have a solid understanding of how to deploy applications, configure ingress, scale your apps, and manage resources. Let's dive in!

## Part 1: Creating a Kubernetes Cluster on Azure

### Step 1: Install Azure CLI

First, ensure you have the Azure CLI installed. You can download it from the [official Azure CLI page](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

### Step 2: Log in to Azure

Open your terminal and log in to your Azure account:

```sh
az login
```

### Step 3: Create a Resource Group

Create a resource group to hold your Kubernetes cluster:

```sh
az group create --name myResourceGroup --location eastus
```

### Step 4: Create a Kubernetes Cluster

Now, create your Kubernetes cluster using Azure Kubernetes Service (AKS):

```sh
az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --enable-addons monitoring --generate-ssh-keys
```

### Step 5: Get Kubernetes Credentials

Fetch the credentials for your new cluster:

```sh
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

### Step 6: Verify Cluster Connection

Check that you can connect to your cluster:

```sh
kubectl get nodes
```

You should see your node listed. Congratulations, your Kubernetes cluster is up and running!

## Part 2: Deploying and Managing Applications

### 1. Setting Up a Hello World Web App

Create a simple deployment and service for a Hello World web app:

```sh
kubectl create deployment hello-world --image=gcr.io/google-samples/hello-app:1.0
kubectl expose deployment hello-world --type=LoadBalancer --port=8080
```

Check the external IP of your service:

```sh
kubectl get services
```

Access your app using the external IP.

### 2. Setting Up an NGINX Ingress to Proxy the App

First, install the NGINX Ingress Controller:

```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

Create an Ingress resource to proxy your Hello World app:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-world-ingress
spec:
  rules:
  - host: hello-world.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: hello-world
            port:
              number: 8080
```

Apply the Ingress resource:

```sh
kubectl apply -f hello-world-ingress.yaml
```

### 3. Assigning a URL via NGINX Ingress

Update your DNS settings to point `hello-world.example.com` to the external IP of your NGINX Ingress Controller.

### 4. Creating Multiple Apps to Proxy via NGINX Ingress

Create another deployment and service for a second app:

```sh
kubectl create deployment another-app --image=nginx
kubectl expose deployment another-app --type=ClusterIP --port=80
```

Update your Ingress resource to include the new app:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-world-ingress
spec:
  rules:
  - host: hello-world.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: hello-world
            port:
              number: 8080
  - host: another-app.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: another-app
            port:
              number: 80
```

Apply the updated Ingress resource:

```sh
kubectl apply -f hello-world-ingress.yaml
```

### 5. Using Azure to Configure the Number of Nodes

Scale your AKS cluster to add more nodes:

```sh
az aks scale --resource-group myResourceGroup --name myAKSCluster --node-count 3
```

### 6. Scaling the Application Using Pods

Scale your Hello World deployment to run multiple pods:

```sh
kubectl scale deployment hello-world --replicas=3
```

### 7. Assigning Resources Like CPU and Memory to Limit the Capacity of Pods

Update your deployment to set resource limits:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world
spec:
  replicas: 3
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
      - name: hello-world
        image: gcr.io/google-samples/hello-app:1.0
        resources:
          limits:
            cpu: "500m"
            memory: "512Mi"
          requests:
            cpu: "250m"
            memory: "256Mi"
```

Apply the updated deployment:

```sh
kubectl apply -f hello-world-deployment.yaml
```

## Conclusion

Congratulations! You've successfully set up a Kubernetes cluster on Azure, deployed a Hello World web app, configured NGINX Ingress, scaled your app, and managed resources. Kubernetes is a powerful tool, and this is just the beginning. Keep exploring and building!

Happy Kubernetes-ing! 🚀
