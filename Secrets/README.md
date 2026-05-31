# 🔐 Kubernetes Secrets Demo (YAML-Based)

This project demonstrates how to:

* Create a **Secret using YAML**
* Use Secret in a Pod as:

  * Environment variables
  * Mounted files (volume)
* Verify Secret inside container

---

# 📁 Project Structure

```
k8s-secret-demo/
├── secret.yaml
├── pod-env.yaml
└── pod-volume.yaml
```

---

# 🧪 Prerequisites

* Minikube running
* kubectl installed

Start cluster:

```bash
minikube start
```

---

# 🔐 STEP 1: Create Secret

Apply:

```bash
kubectl apply -f secret.yaml
```

Verify:

```bash
kubectl get secrets
kubectl describe secret my-secret
```

---

## 🧠 Note

Values in Secret are **base64 encoded**

Example:

```bash
echo -n "admin" | base64
echo -n "1234" | base64
```

---

# 🚀 STEP 2: Use Secret as Environment Variables

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
kubectl exec -it secret-env-demo -- sh
```

Inside container:

```bash
echo $USERNAME
echo $PASSWORD
```

---

## ✅ Expected Output

```
admin
1234
```

---

# 🟪 STEP 3: Use Secret as Volume (File)

Apply:

```bash
kubectl apply -f pod-volume.yaml
```

---

## 🔍 Verify inside container

```bash
kubectl exec -it secret-volume-demo -- sh
```

Inside container:

```bash
cd /etc/secrets
ls
cat username
cat password
```

---

## ✅ Expected Output

```
admin
1234
```

---

# 🧠 How It Works

```
Secret → Pod → Injected as:
                ENV variables OR files
                ↓
            Application uses it
```

---

# ⚠️ Common Issues

* ❌ Secret not created → `kubectl get secrets`
* ❌ Wrong key name in YAML
* ❌ Pod not running → `kubectl describe pod <pod>`
* ❌ Forgot base64 encoding

---

# 🎯 Key Takeaways

* Secrets store **sensitive data securely**
* Keeps credentials **out of code**
* Can be used as:

  * Environment variables
  * Mounted files

---

# 🧠 Interview Insight

👉 Secrets are **base64 encoded (not encrypted)**
👉 Use tools like **Vault / AWS Secrets Manager** in production

---

# 🧹 Cleanup

```bash
kubectl delete -f pod-env.yaml
kubectl delete -f pod-volume.yaml
kubectl delete -f secret.yaml
```

---