# ☸️ Kubernetes Core Concepts & Resource Guide

> **Project Scope:** `vgdorder.online`  
> **Environment:** Production-Ready Configuration  

---

## 📑 Table of Contents
1. [🏗️ Phase 1: Governance & Resource Control](#️-phase-1-governance--resource-control)
2. [📦 Phase 2: Workloads & Scaling](#-phase-2-workloads--scaling)
3. [🌐 Phase 3: Networking & Advanced Workloads](#-phase-3-networking--advanced-workloads)
4. [💾 Phase 4: Persistence & Configuration](#-phase-4-persistence--configuration)
5. [🛡️ Phase 5: RBAC (Access Control)](#️-phase-5-rbac-access-control)

---

## 🏗️ Phase 1: Governance & Resource Control

| File | Resource Name | Key Capabilities |
| :--- | :--- | :--- |
| **01.ns.yml** | **Namespace** | **Isolation:** Virtual clusters for project separation.<br>**Security:** Scope for RBAC.<br>**Management:** Prevent cross-project interference. |
| **02.rq.yml** | **Resource Quota** | **Hard Limits:** Max CPU/RAM per namespace.<br>**Object Counting:** Limits total Pods/Services.<br>**Stability:** Prevents "Noisy Neighbor" syndrome. |
| **03.lr.yml** | **Limit Range** | **Defaulting:** Auto-assigns CPU/RAM if missing.<br>**Constraints:** Enforces Min/Max pod sizes.<br>**Granularity:** Works at the container level. |

---

## 📦 Phase 2: Workloads & Scaling

### 📄 04.po.yml — Pod
* **Smallest Unit:** The basic building block that runs your container.
* **Ephemeral:** Designed to be temporary; shares network and storage.

### 📄 05.rc.yml — Replication Controller
* **Legacy Tool:** Older version of ReplicaSet.
* **Desired State:** Ensures "X" number of Pods are always running.
* **Self-Healing:** Automatically replaces crashed Pods.

### 📄 06.rs.yml — ReplicaSet
* **Modern Scaling:** Successor to RC with better "Label Selectors."
* **Grouping:** Uses complex logic to manage Pod sets.
* **Deployment Component:** Usually managed by a Deployment.

### 📄 07.dep.yml — Deployment
* **Management:** The standard way to deploy `vgdorder.online`.
* **Rolling Updates:** Zero-downtime application updates.
* **Rollbacks:** Quickly revert to previous stable versions.

---

## 🌐 Phase 3: Networking & Advanced Workloads

### 📄 08.svc.yml — Service
* **Stable IP:** Permanent entry point for pods with shifting IPs.
* **Load Balancing:** Distributes traffic across healthy replicas.
* **External Access:** `type: LoadBalancer` creates a cloud entry point (ELB).

### 📄 09.hpa.yml — Horizontal Pod Autoscaler
* **Elasticity:** Scales Pod count based on CPU/RAM usage spikes.
* **Cost Saving:** Scales down during low traffic to save money.
* **Metrics-Based:** Requires a metrics-server in the cluster.

### 📄 10.ds.yml — DaemonSet
* **One-Per-Node:** Runs exactly one copy of a Pod on every worker node.
* **Infrastructure:** Best for logs (Fluentd) or monitoring (Datadog).
* **Auto-Deployment:** Automatically starts on any new node added.

---

## 💾 Phase 4: Persistence & Configuration

### 🗄️ Storage Logic
* **11.pv.yml (Persistent Volume):** The actual physical disk. **Global resource**.
* **12.pvc.yml (Persistent Volume Claim):** The request form for storage. **Namespace-bound**.

### ⚙️ Configuration Logic
* **13.cm.yml (ConfigMap):** Stores non-sensitive settings (URLs, Ports).
* **14.sec.yml (Secret):** Stores sensitive data (Passwords, Keys). Base64 encoded and stored in RAM.

---

## 🛡️ Phase 5: RBAC (Access Control)

| Type | Resource | Scope | Description |
| :--- | :--- | :--- | :--- |
| **Local** | **15.ro.yml (Role)** | Namespace | Defines allowed actions (get, list) in one area. |
| **Local** | **16.rob.yml (RoleBinding)** | Namespace | Links a Role to a User (e.g., Jagadish). |
| **Global** | **17.cr.yml (ClusterRole)** | Cluster | Permissions for global resources (Nodes/PVs). |
| **Global** | **18.crb.yml (ClusterRoleBinding)** | Cluster | Grants Admin power across the entire cluster. |

---

[⬅️ Back to Top](#️-kubernetes-core-concepts--resource-guide)
