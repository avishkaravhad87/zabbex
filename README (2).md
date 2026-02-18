# Zabbix Deployment on Kubernetes (Manual YAML -- KillerCoda Lab)

## 📌 Overview

This project demonstrates a manual deployment of Zabbix 6.0 on
Kubernetes using YAML manifests (without Helm).

Components deployed:

-   MariaDB 10.5 (Database)
-   Zabbix Server (Backend Engine)
-   Zabbix Web (Frontend UI)
-   Kubernetes Services (ClusterIP + NodePort)
-   Internal Kubernetes DNS communication

Environment used: KillerCoda Kubernetes Lab

------------------------------------------------------------------------

## 🏗 Architecture

### Components

1.  MariaDB 10.5
    -   Stores monitoring data
    -   Stores configuration, triggers, users, and history
2.  Zabbix Server
    -   Core monitoring engine
    -   Processes metrics
    -   Evaluates triggers
    -   Sends alerts
3.  Zabbix Web
    -   Frontend dashboard
    -   Used for configuration and visualization
4.  Kubernetes Services
    -   ClusterIP → internal communication
    -   NodePort → external access

------------------------------------------------------------------------

## 🔄 Data Flow

Target System → Zabbix Agent → Zabbix Server → MariaDB → Zabbix Web UI

------------------------------------------------------------------------

## 🚀 Deployment Steps

### 1️⃣ Create Namespace

kubectl create namespace monitoring

------------------------------------------------------------------------

### 2️⃣ Deploy MariaDB

kubectl apply -f mysql-deployment.yaml\
kubectl apply -f mysql-service.yaml

------------------------------------------------------------------------

### 3️⃣ Deploy Zabbix Server

kubectl apply -f zabbix-server.yaml

------------------------------------------------------------------------

### 4️⃣ Deploy Zabbix Web

kubectl apply -f zabbix-web.yaml\
kubectl apply -f zabbix-web-service.yaml

------------------------------------------------------------------------

### 5️⃣ Create Zabbix Server Service

kubectl apply -f zabbix-server-service.yaml

------------------------------------------------------------------------

### 6️⃣ Access in KillerCoda

1.  Open Traffic Port Accessor\
2.  Enter NodePort (example: 30007)\
3.  Click Access

Login Credentials:

Username: Admin\
Password: zabbix

------------------------------------------------------------------------

## 🧯 Errors Faced & Troubleshooting

### ❌ ImagePullBackOff

Cause: Incorrect image tag\
Fix: Use valid tags like\
zabbix/zabbix-server-mysql:6.0-ubuntu-latest

------------------------------------------------------------------------

### ❌ CrashLoopBackOff

Cause: Database schema not initialized properly\
Fix: Drop database and restart Zabbix Server

------------------------------------------------------------------------

### ❌ MySQL 8 Compatibility Issue

Problem: Schema import conflicts\
Solution: Use mariadb:10.5

------------------------------------------------------------------------

### ❌ Database Collation Warning

Warning about utf8mb4_general_ci\
Fix (Production):\
CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;

------------------------------------------------------------------------

## 🔎 Useful Debugging Commands

Check pods: kubectl get pods -n monitoring

Check logs: kubectl logs -n monitoring deployment/zabbix-server

Access MySQL: kubectl exec -it `<mysql-pod>`{=html} -n monitoring --
mysql -u root -p

------------------------------------------------------------------------

## 🎯 Key Learning Outcomes

-   Debugging ImagePullBackOff
-   Debugging CrashLoopBackOff
-   Understanding NodePort vs ClusterIP
-   Kubernetes DNS communication
-   Database schema initialization
-   Version compatibility troubleshooting
-   Manual 3-tier deployment on Kubernetes

------------------------------------------------------------------------

## 🏆 Final Status

All components running successfully:

-   MariaDB
-   Zabbix Server
-   Zabbix Web

Zabbix Dashboard accessible via NodePort in KillerCoda.

------------------------------------------------------------------------

Author: Manual Kubernetes deployment and troubleshooting performed in
lab environment.
