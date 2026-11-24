# Task 4 - Kubernetes Autoscaling (HPA)

## Goal

Automatically scale the TTT app when CPU usage increases.

- Minimum: 2 app Pods
- Maximum: 10 app Pods
- MongoDB stays at 1 Pod

## What is HPA?

HPA means Horizontal Pod Autoscaler.

It adds more Pods when the app is busy and reduces them when demand drops.

## Step 1 - Add CPU Resources

In `ttt-deploy.yml`, under the app container, I added:

```yaml
resources:
  requests:
    cpu: "100m"
    memory: "128Mi"
  limits:
    cpu: "500m"
    memory: "256Mi"
```

- `requests` - Resources the app needs.
- `limits` - Maximum resources the container can use.
- HPA uses the CPU request to calculate CPU usage.

## Step 2 - Create the HPA

I created `ttt-hpa.yml`:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: ttt-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: ttt-deployment
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
```

- `minReplicas: 2` - At least 2 app Pods.
- `maxReplicas: 10` - No more than 10 app Pods.
- `averageUtilization: 50` - Target CPU usage is 50% of the requested CPU.

## Step 3 - Check Metrics Server

HPA needs Metrics Server to read CPU usage.

```bash
kubectl top pods
```

I initially got `Metrics API not available`.

I installed Metrics Server:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

My local Kubernetes cluster had a certificate error, so I used this local testing fix:

```bash
kubectl patch deployment metrics-server -n kube-system --type=json -p='[{"op":"add","path":"/spec/template/spec/containers/0/args/-","value":"--kubelet-insecure-tls"}]'
```

This skips certificate verification and is only suitable for local testing.

## Step 4 - Apply the YAML Files

```bash
kubectl apply -f ttt-deploy.yml
kubectl apply -f ttt-hpa.yml
```

Check the HPA:

```bash
kubectl get hpa
```

## Step 5 - Generate Traffic

Forward the app port:

```bash
kubectl port-forward service/ttt-service 3002:3000
```

In another terminal, run Apache Bench:

```bash
ab -n 100000 -c 100 http://localhost:3002/
```

- `-n 100000` - Send 100,000 requests.
- `-c 100` - Send up to 100 requests at the same time.

## Step 6 - Watch Autoscaling

In another terminal:

```bash
kubectl get hpa -w
```

- `TARGETS` - Current CPU usage / target CPU usage.
- `MINPODS` - Minimum Pods.
- `MAXPODS` - Maximum Pods.
- `REPLICAS` - Current app replicas.
- `-w` - Watch for changes.

Check the Pods:

```bash
kubectl get pods
```