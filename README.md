# Zabbix Deployment on Kubernetes (Manual YAML - KillerCoda Lab)

## 📌 Overview

This project documents the manual deployment of Zabbix 6.0 on Kubernetes using:

- MariaDB (Database)
- Zabbix Server
- Zabbix Web (Frontend)
- NodePort Service
- Kubernetes DNS for internal communication

Environment: KillerCoda Kubernetes Lab

---

## 🏗 Architecture

Zabbix Components:

1. MariaDB (Database)
2. Zabbix Server (Backend / Processing Engine)
3. Zabbix Web (UI)
4. Kubernetes Services (ClusterIP + NodePort)

Data Flow:

Target → Zabbix Agent → Zabbix Server → MariaDB → Zabbix Web UI

---

## 🚀 Deployment Steps

### 1️⃣ Create Namespace

```bash
kubectl create namespace monitoring
2️⃣ Deploy MariaDB

Image: mariadb:10.5

Database: zabbix

User: zabbix

Password: zabbixpassword

kubectl apply -f mysql-deployment.yaml
kubectl apply -f mysql-service.yaml

3️⃣ Deploy Zabbix Server
kubectl apply -f zabbix-server.yaml

4️⃣ Deploy Zabbix Web
kubectl apply -f zabbix-web.yaml
kubectl apply -f zabbix-web-service.yaml

5️⃣ Create Zabbix Server Service (Important)
apiVersion: v1
kind: Service
metadata:
  name: zabbix-server
  namespace: monitoring
spec:
  ports:
    - port: 10051
      targetPort: 10051
  selector:
    app: zabbix-server

kubectl apply -f zabbix-server-service.yaml

🌐 Accessing in KillerCoda

Use Traffic Port Accessor:

Enter NodePort: 30007

Click Access

Login:

Username: Admin
Password: zabbix

🧯 Errors Faced & Troubleshooting
❌ 1. ImagePullBackOff
Error:
Failed to pull image: zabbix/zabbix-server-mysql:6.0-latest not found

Cause:

Incorrect Docker image tag.

Fix:

Use correct tag:

zabbix/zabbix-server-mysql:6.0-ubuntu-latest
zabbix/zabbix-web-nginx-mysql:6.0-ubuntu-latest

❌ 2. CrashLoopBackOff (Database Error)
Error:
cannot use database "zabbix": its "users" table is empty

Cause:

Database partially initialized

Schema corrupted

Manual import conflict

Fix:

Drop database completely

Restart Zabbix server
OR

Recreate database cleanly

❌ 3. MySQL 8 Compatibility Issue
Error:

Schema import failed
Table already exists
Initialization inconsistent

Cause:

Zabbix 6.0 not fully compatible with MySQL 8 default configuration.

Fix:

Switch to:

mariadb:10.5


Recommended for Zabbix 6.0.

❌ 4. Database Collation Warning
Warning:
Zabbix supports only utf8mb4_bin
Database has utf8mb4_general_ci

Impact:

Non-fatal warning.

Production Fix:

Create database using:

CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;

❌ 5. "Zabbix server is not running" in UI
Cause:

Zabbix Server service not created.

Fix:

Create ClusterIP service for port 10051.

❌ 6. Unable to Select Configuration (UI Error)
Cause:

Database schema not properly initialized.

Fix:

Drop DB

Restart server

Ensure MariaDB 10.5 is used

🧠 Debugging Commands Used

Check Pods:

kubectl get pods -n monitoring


Describe Pod:

kubectl describe pod <pod-name> -n monitoring


Check Logs:

kubectl logs -n monitoring deployment/zabbix-server


Drop Database:

kubectl exec -it <mysql-pod> -n monitoring -- mysql -u root -p

🎯 Key Learning Outcomes

Debugging ImagePullBackOff

Debugging CrashLoopBackOff

Understanding Kubernetes Services (ClusterIP vs NodePort)

Internal DNS resolution

Database schema initialization issues

Version compatibility troubleshooting

Zabbix 3-tier architecture on Kubernetes

🏆 Final Status

All pods running:

MariaDB

Zabbix Server

Zabbix Web

Zabbix Dashboard accessible via NodePort in KillerCoda.

👨‍💻 Author

Manual Kubernetes deployment and troubleshooting performed in lab environment.


---

🔥 This README is interview-ready and GitHub-ready.

If you want, I can now:

- Make it more professional (resume-level)
- Add architecture diagram
- Convert into DevOps portfolio project format
- Or help you push it to your GitHub properly
