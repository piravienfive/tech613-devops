# Kubernetes - Beginner Notes

## What is Kubernetes?

Kubernetes (K8s) is a tool that automatically manages containers.

**Why use it?**
- **Scaling:** Run more containers.
- **Self-healing:** Replace failed Pods.
- **Automation:** Manage applications automatically.
- **Availability:** Help keep applications running.

## Basic Terms

- **Cluster:** Group of machines.
- **Node:** Machine running applications.
- **Pod:** Runs one or more containers.
- **Deployment:** Manages Pods and replicas.
- **Service:** Allows access to Pods.
- **YAML:** File containing Kubernetes instructions.
- **kubectl:** Tool used to control Kubernetes.

## Basic Kubernetes Commands

### View Resources

`kubectl get all`
- Shows common resources like Pods, Deployments and Services.

`kubectl get pods`
- Shows running and pending Pods.

`kubectl get services`
- Shows Services and their ports.

### Create Resources

`kubectl apply -f nginx-deploy.yml`
- Creates or updates the Nginx Deployment using YAML.
- Kubernetes creates the required Pods.

### Edit Resources

`kubectl edit deploy nginx-deployment`
- Opens the Deployment configuration for editing.
- Can change replicas, images and other settings.

### Delete Resources

`kubectl delete -f nginx-deploy.yml`
- Deletes resources defined in the Deployment YAML.

`kubectl delete -f nginx-service.yml`
- Deletes resources defined in the Service YAML.

## Simple Workflow

1. `kubectl apply -f nginx-deploy.yml` - Deploy Nginx.
2. `kubectl get pods` - Check Pods.
3. `kubectl get all` - Check resources.
4. `kubectl get services` - Check networking.
5. `kubectl edit deploy nginx-deployment` - Make changes.
6. `kubectl delete -f nginx-deploy.yml` - Remove Deployment.
7. `kubectl delete -f nginx-service.yml` - Remove Service.

## Quick Revision

- **apply:** Create or update.
- **get:** View resources.
- **edit:** Modify resources.
- **delete:** Remove resources.
- **Deployment:** Manages Pods.
- **Service:** Provides network access to Pods.
