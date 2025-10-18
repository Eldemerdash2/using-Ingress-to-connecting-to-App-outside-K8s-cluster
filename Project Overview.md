# 🧭 Ingress – Connecting to Applications Outside a Kubernetes Cluster

![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.29-blue?logo=kubernetes&logoColor=white)
![Ingress](https://img.shields.io/badge/Ingress-Controller-brightgreen?logo=nginx)
![License](https://img.shields.io/badge/License-MIT-yellow)
![Platform](https://img.shields.io/badge/Platform-Minikube-orange?logo=minikube)
![Status](https://img.shields.io/badge/Status-Active-success)

> Learn how to expose your internal Kubernetes services securely using **Ingress**, configure HTTPS, and route traffic via domain names.

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

### 🔸 2. Ingress Resource Example

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: dashboard-ingress
  namespace: kubernetes-dashboard
spec:
  rules:
  - host: dashboard.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: kubernetes-dashboard
            port:
              number: 80
```

🧩 This rule forwards all traffic from `dashboard.com` to the **Kubernetes Dashboard service**.

---

## 🚀 Setting Up Ingress in Minikube

### ▶️ Enable Ingress Controller
```bash
minikube addons enable ingress
```

### 🛠️ Start a Tunnel
```bash
minikube tunnel
```

### 🧾 Map Local Hostname
```bash
sudo nano /etc/hosts
# Add this line:
127.0.0.1   dashboard.com
```

Now open your browser → **[http://dashboard.com](http://dashboard.com)** 🎉

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

## 🧩 Advanced Use Cases

### 🔹 Multiple Paths (Same Domain)
```yaml
- host: myapp.com
  http:
    paths:
    - path: /analytics → analytics-service
    - path: /shop      → shop-service
```
📊 One domain → Multiple paths → Different apps.

---

### 🔹 Multiple Subdomains
```yaml
- host: analytics.myapp.com → analytics-service
- host: shop.myapp.com      → shop-service
```
🏷️ Use **subdomains** instead of path routing.

---

### 🔹 Default Backend
Define a **fallback service** for unmapped requests (custom 404/error page).
```yaml
defaultBackend:
  service:
    name: error-page
    port:
      number: 80
```

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
| 5️⃣ | `echo "127.0.0.1 dashboard.com" | sudo tee -a /etc/hosts` | Map hostname |
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
🎓 *Kubernetes Learning Series – Lesson 130: Ingress: Connecting to Applications Outside the Cluster*  

💬 *Learn. Build. Automate.*  
🖋️ [GitHub Profile](https://github.com/) • 🌐 [Kubernetes Docs](https://kubernetes.io)
