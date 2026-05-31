# 🐳 Kubernetes Services Demo (ClusterIP, NodePort, LoadBalancer)

This guide covers **3 types of Kubernetes Services** with **step-by-step commands and simple explanations**.

---

# 🧠 What is a Service?

👉 A Service exposes your application (pods) in Kubernetes.

```text
Pods are dynamic → Service gives stable access
```

---

# 🧪 Prerequisite

```bash
minikube start
kubectl get nodes
```

---

# 🟦 1. ClusterIP (Internal Communication)

---

## 🎯 Use Case

👉 Communication **inside the cluster only**

---

## 🔹 Step 1: Create Deployment

```bash
kubectl create deployment backend --image=nginx
```

---

## 🔹 Step 2: Expose as ClusterIP

```bash
kubectl expose deployment backend --port=80 --target-port=80 --type=ClusterIP
```

---

## 🔹 Step 3: Verify

```bash
kubectl get svc
```

---

👉 Output:

```text
backend   ClusterIP   10.96.x.x
```

---

## ❌ Try in browser

```text
http://10.96.x.x ❌ (won’t work)
```

---

## 🔹 Step 4: Test Inside Cluster

```bash
kubectl run test --image=busybox --restart=Never -it --rm -- sh
```

Inside pod:

```bash
wget -qO- backend
```

---

## 🧠 Explanation

```text
Pod → Service → Pod
```

✔ Works internally
❌ Not accessible externally

---

---

# 🌐 2. NodePort (External Access - Basic)

---

## 🎯 Use Case

👉 Access app from **outside cluster (local testing)**

---

## 🔹 Step 1: Create Deployment

```bash
kubectl create deployment nodeport-demo --image=nginx
```

---

## 🔹 Step 2: Expose as NodePort

```bash
kubectl expose deployment nodeport-demo --type=NodePort --port=80
```

---

## 🔹 Step 3: Get Service

```bash
kubectl get svc
```

---

👉 Output:

```text
nodeport-demo   NodePort   80:30007
```

---

## 🔹 Step 4: Get Minikube IP

```bash
minikube ip
```

---

## 🔹 Step 5: Access

```text
http://<minikube-ip>:30007
```

---

## ❗ If Minikube IP not working → use port-forward

```bash
kubectl port-forward svc/nodeport-demo 8080:80
```

Then open:

```text
http://localhost:8080
```

---

## 🧠 Explanation

```text
User → NodeIP:Port → Service → Pods
```

---

---

# ☁️ 3. LoadBalancer (Production Style)

---

## 🎯 Use Case

👉 Production apps (cloud-based access)

---

## 🔹 Step 1: Create Deployment

```bash
kubectl create deployment lb-demo --image=nginx
```

---

## 🔹 Step 2: Expose as LoadBalancer

```bash
kubectl expose deployment lb-demo --type=LoadBalancer --port=80
```

---

## 🔹 Step 3: Check Service

```bash
kubectl get svc
```

---

👉 Output:

```text
lb-demo   LoadBalancer   <pending>
```

---

## 🔹 Step 4: Start Tunnel (IMPORTANT)

```bash
minikube tunnel
```

---

## 🔹 Step 5: Check Again

```bash
kubectl get svc
```

---

👉 Output:

```text
lb-demo   LoadBalancer   127.0.0.1
```

---

## 🔹 Step 6: Access

```text
http://127.0.0.1
```

---

## 🧠 Explanation

```text
User → LoadBalancer → NodePort → Service → Pods
```

---

---

# 🔁 Scaling Demo (Works for All)

```bash
kubectl scale deployment backend --replicas=3
kubectl get pods
```

---

# 💥 Self-Healing Demo

```bash
kubectl delete pod <pod-name>
kubectl get pods
```

👉 Pod recreated automatically

---

---

# 🧠 Final Comparison

| Type         | Access              | Use Case      |
| ------------ | ------------------- | ------------- |
| ClusterIP    | Internal            | Microservices |
| NodePort     | External (basic)    | Local testing |
| LoadBalancer | External (advanced) | Production    |

---

# 🎯 Key Takeaways

👉 ClusterIP = internal communication
👉 NodePort = external access via node
👉 LoadBalancer = production-grade access

---

# 💡 One-Line Summary

👉
**Services expose your application at different levels depending on where you want access—from inside the cluster to the public internet**

---