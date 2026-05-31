# ⚙️ Kubernetes ConfigMap Demo (Step-by-Step)

This demo shows how to:

* Create a **ConfigMap using YAML**
* Use ConfigMap in a Pod as:

  * Environment variables
  * Mounted files
* Verify inside container

---

# 📁 Project Structure

```
k8s-configmap-demo/
├── configmap.yaml
├── pod-env.yaml
└── pod-volume.yaml
```

---

# 🧪 Prerequisites

* Minikube installed
* kubectl installed

Start cluster:

```bash
minikube start
```

---

# ⚙️ STEP 1: Create ConfigMap

Apply:

```bash
kubectl apply -f configmap.yaml
```

Verify:

```bash
kubectl get configmap
kubectl describe configmap app-config
```

---

# 🚀 STEP 2: Use ConfigMap as Environment Variables

Apply:

```bash
kubectl apply -f pod-env.yaml
```

Check pod:

```bash
kubectl get pods
```

---

## 🔍 Verify inside container

```bash
kubectl exec -it configmap-env-demo -- sh
```

Inside container:

```bash
echo $APP_MODE
echo $DB_HOST
```

---

## ✅ Expected Output

```
production
mysql
```

---

# 🟪 STEP 3: Use ConfigMap as Volume (File)

Apply:

```bash
kubectl apply -f pod-volume.yaml
```

---

## 🔍 Verify inside container

```bash
kubectl exec -it configmap-volume-demo -- sh
```

Inside container:

```bash
cd /etc/config
ls
cat APP_MODE
cat DB_HOST
```

---

## ✅ Expected Output

```
production
mysql
```

---

# 🧠 How It Works

```
ConfigMap → Pod → Injected as:
                 ENV variables OR files
                 ↓
             Application reads config
```

---

# ⚠️ Common Issues

* ❌ ConfigMap not applied → `kubectl get configmap`
* ❌ Wrong key name
* ❌ Pod not running → `kubectl describe pod <pod>`
* ❌ Changes not reflected → recreate pod

---

# 🎯 Key Takeaways

* ConfigMap stores **non-sensitive configuration**
* Keeps config **separate from code**
* Can be used as:

  * Environment variables
  * Mounted files

---

# 🧠 Interview Insight

👉 ConfigMap is used for **configuration only**
👉 Use **Secret for sensitive data (passwords, tokens)**

---

# 🧹 Cleanup

```bash
kubectl delete -f pod-env.yaml
kubectl delete -f pod-volume.yaml
kubectl delete -f configmap.yaml
```

---