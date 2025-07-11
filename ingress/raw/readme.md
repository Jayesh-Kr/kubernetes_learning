# Kubernetes Nginx Reverse Proxy Example

This guide will help you set up a local Kubernetes cluster using kind, deploy two web applications, expose them via Services, and route traffic using an Nginx reverse proxy.

---

## Prerequisites

- Docker installed
- kind installed
- kubectl installed

---

## Steps

### 1. Create a kind Cluster

Edit `clusters.yml` to configure your cluster and port mappings.  
Create the cluster:

```sh
kind create cluster --config clusters.yml
```

---

### 2. Deploy Applications

Apply the deployments for Apache and Httpd:

```sh
kubectl apply -f deployment.yml
```

---

### 3. Create Services

Apply the service definitions for both apps:

```sh
kubectl apply -f services.yml
```

---

### 4. Set Up Nginx Reverse Proxy

Apply the ConfigMap, Deployment, and Service for Nginx:

```sh
kubectl apply -f reverseproxy.yml
```

---

### 5. Update Your Hosts File (Windows)

Add these lines to `C:\Windows\System32\drivers\etc\hosts`:

```
127.0.0.1 apache.jayesh.com
127.0.0.1 httpd.jayesh.com
```

---

### 6. Test in Browser

Open your browser and visit:

- [http://apache.jayesh.com:30007](http://apache.jayesh.com:30007)  
  (routes to Apache backend)
- [http://httpd.jayesh.com:30007](http://httpd.jayesh.com:30007)  
  (routes to Httpd backend)

---

## How It Works

- Deployments create Pods for Apache and Httpd.
- Services expose these Pods on cluster ports.
- Nginx reverse proxy (as ingress) routes traffic based on domain name.
- NodePort exposes Nginx to your local machine.

---

## Cleanup

To delete everything:

```sh
kubectl delete -f reverseproxy.yml
kubectl delete -f services.yml
kubectl delete -f deployment.yml
kind delete cluster
```

---