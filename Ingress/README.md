# 🌐 Kubernetes Ingress Demo (Minikube)

This guide shows how to:

* Enable Ingress
* Deploy apps
* Apply Ingress rules
* Access using port-forward

---

# 🧪 Prerequisites

```bash
minikube start
kubectl get nodes
```

---

# 🚀 STEP 1: Enable Ingress Controller

```bash
minikube addons enable ingress
```

Verify:

```bash
kubectl get pods -n ingress-nginx
```

👉 Wait until:

```
Running ✅
```

---

# 🏗️ STEP 2: Create Applications

```bash
kubectl create deployment app1 --image=nginx
kubectl expose deployment app1 --port=80

kubectl create deployment app2 --image=httpd
kubectl expose deployment app2 --port=80
```

---

# 🌐 STEP 3: Create Ingress YAML

Create file:

```bash
vi ingress.yaml
```

---

# 🚀 STEP 4: Apply Ingress

```bash
kubectl apply -f ingress.yaml
```

Verify:

```bash
kubectl get ingress
```

---

# 🔗 STEP 5: Port Forward Ingress (IMPORTANT)

```bash
kubectl port-forward -n ingress-nginx svc/ingress-nginx-controller 8080:80
```

---

# 🌍 STEP 6: Access Applications

Open in browser:

```
http://localhost:8080/app1
http://localhost:8080/app2
```

---

# 🧠 How It Works

```
Browser → Ingress → Service → Pods
```

* `/app1` → routes to app1
* `/app2` → routes to app2

---


# 🎯 Summary

👉 Ingress provides:

* Single entry point
* Path-based routing
* Cleaner alternative to NodePort

---