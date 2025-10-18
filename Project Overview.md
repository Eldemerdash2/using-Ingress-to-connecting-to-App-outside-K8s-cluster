# 🧭 Ingress – Connecting to Applications Outside a Kubernetes Cluster

---

## 🧠 Overview

In **Kubernetes**, an **Ingress** is a resource that manages **external access** to internal services, usually over **HTTP/HTTPS**.  
Instead of relying on `NodePort` or `LoadBalancer`, Ingress allows clean, domain-based and secure routing.

### ✨ Benefits
- 🌐 Domain-based access (e.g., `myapp.com`)
- 🔒 HTTPS/TLS encryption
- 🎯 Centralized routing rules
- ⚡ Fine-grained control via paths or subdomains

---

## 🏗️ Architecture

```text
🌍 User Browser
       │
       ▼
🚪 Ingress Controller  ← (Cluster Entry Point)
       │
       ▼
📜 Ingress Resource → Applies routing rules
       │
       ▼
🔁 Internal Service (ClusterIP)
       │
       ▼
📦 Application Pod
```

---

## ⚙️ Key Concepts

### 🔸 1. External vs Internal Services

| Type | Description | Example |
|------|--------------|----------|
| **External (LoadBalancer / NodePort)** | Opens the app publicly using node IP and port – quick but insecure. | `http://NodeIP:30000` |
| **Internal (ClusterIP)** | Used behind Ingress for secure internal routing. | Access via domain (e.g., `myapp.com`) |


---


## 🔒 Enabling HTTPS (TLS)

To secure your application with HTTPS:

### 1️⃣ Create a TLS Secret
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: tls-secret
  namespace: kubernetes-dashboard
type: kubernetes.io/tls
data:
  tls.crt: <base64_encoded_cert>
  tls.key: <base64_encoded_key>
```

### 2️⃣ Reference the Secret in Ingress
```yaml
spec:
  tls:
  - hosts:
    - dashboard.com
    secretName: tls-secret
```

🔐 Done! Your application now supports **HTTPS connections**.


---

## ☁️ Deployment Scenarios

| 🏢 Environment | 🌐 Entry Point | 🧭 Description |
|----------------|----------------|----------------|
| **Cloud (AWS / GCP / Linode)** | Cloud Load Balancer → Ingress Controller | Easiest to set up using managed load balancers. |
| **Bare Metal** | External Proxy Server (e.g., Nginx, HAProxy) → Ingress Controller | Manual setup for maximum control and security. |

---

## 🧑‍💻 Demo Steps (Minikube)

| Step | Command | Description |
|------|----------|-------------|
| 1️⃣ | `minikube start` | Start your cluster |
| 2️⃣ | `minikube addons enable ingress` | Enable ingress addon |
| 3️⃣ | `minikube dashboard` | Launch the Kubernetes dashboard |
| 4️⃣ | `kubectl apply -f dashboard-ingress.yaml` | Create ingress rule |
| 5️⃣ | `add 127.0.0.1 dashboard.com in /etc/hosts" | map domain name dashboard.com to 127.0.0.1 in /etc/hosts |
| 6️⃣ | `minikube tunnel` | Start the ingress tunnel |
| ✅ | Visit [http://dashboard.com](http://dashboard.com) | Access your dashboard! |

---

## 🧾 Quick Summary

| 🧩 Concept | 📘 Description |
|------------|----------------|
| **Ingress** | Routes external HTTP/S traffic to internal services |
| **Ingress Controller** | Processes and enforces ingress rules |
| **ClusterIP Service** | Internal-only service used by Ingress |
| **TLS Secret** | Stores SSL cert & key for HTTPS |
| **Default Backend** | Handles unmatched routes or error responses |

---

## 📚 References

📎 **Official Documentation:**
- [Kubernetes Ingress Concepts](https://kubernetes.io/docs/concepts/services-networking/ingress/)
- [NGINX Ingress Controller](https://kubernetes.github.io/ingress-nginx/)
- [Minikube Addons Guide](https://minikube.sigs.k8s.io/docs/handbook/addons/)

---

## 👨‍💻 Author

**Mohamed Eldemerdash**  
DevOps Engineer & Mechanical Engineer

