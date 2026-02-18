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
