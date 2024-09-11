# Kubernetes Multi-Container Pod with NGINX Firewall

![Markdown](https://github.com/hasanashik/yaml-learning/blob/main/01%20Pod%20with%20Inter-Container%20Communication%20and%20Firewall-Proxy%20Setup/diagram.PNG?style=for-the-badge&logo=markdown&logoColor=white)

This project demonstrates how to deploy a Kubernetes Pod with three containers on Minikube, using NGINX for Layer 7 (L7) load balancing. The pod includes a **firewall-container** that proxies requests to two backend containers: **container-1** and **container-2**. All traffic is routed through the firewall-container, abstracting direct access to the backend services.

## Objectives

- Deploy a multi-container Pod in Minikube with three containers:
  - `firewall-container` (acts as a reverse proxy)
  - `container-1` (NGINX-based service)
  - `container-2` (NGINX-based service)
- Implement Layer 7 (L7) load balancing using NGINX in the firewall-container.
- Route all requests through the firewall-container.

![Markdown](Live demo-1.gif?style=for-the-badge&logo=markdown&logoColor=white)

## Requirements

- Windows 10 PC
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [Docker Desktop](https://docs.docker.com/desktop/install/windows-install/)

Follow the installation instructions for Minikube and Docker Desktop from the provided links.

## Getting Started

1. **Start Minikube**  
   Open a terminal or PowerShell and run:
   ```bash
   minikube start
   ```
2. **Apply Pod Configuration**  
   Clone the repository and navigate to the directory containing the pod.yaml file. Run the following command to create the pod:

```bash
kubectl apply -f pod.yaml
```

3. **Create a Tunnel to Minikube**
   Set up a network tunnel between your Windows environment and the Minikube cluster:

```bash
minikube tunnel
```

4. **Access the Services**
   Access the firewall service from your browser:

```bash
minikube service firewall-service
```

You can now reach the backend services through the firewall:

Container-1: http://127.0.0.1:8195/app1
Container-2: http://127.0.0.1:8195/app2

## Pod Structure Overview

container-1 and container-2 are NGINX containers that serve custom HTML content.
firewall-container is an NGINX proxy that load balances requests between the two backend containers.
The ConfigMap provides the NGINX configuration for the firewall-container, handling the routing and error pages.
Useful Commands
Check Pod Status:

```bash
kubectl get pods
```

Check Services:

```bash
kubectl get svc
```

Access a Specific Container:

```bash
kubectl exec -it firewall-pod --container container-1 -- /bin/sh
```

View Logs:

```bash
kubectl logs firewall-pod -c firewall-container
```

Cleanup
To delete the pod and service, run:

```bash
kubectl delete pod firewall-pod
kubectl delete svc firewall-service
```
