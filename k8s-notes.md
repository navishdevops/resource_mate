# ☸️ Kubernetes Core Concepts & Resource Guide

> **Project Scope:** `vgdorder.online`  
> **Environment:** Production-Ready Configuration  

---

## 🏛️ Phase 1: Governance & Resource Control

| File | Resource Name | Key Capabilities |
| :--- | :--- | :--- |
| **01.ns.yml** | **Namespace** | **Isolation:** Creates a "virtual cluster" to keep projects separate.<br>**Security:** Scope for RBAC permissions.<br>**Management:** Prevent cross-project resource interference. |
| **02.rq.yml** | **Resource Quota** | **Hard Limits:** Max CPU/RAM per namespace.<br>**Object Counting:** Limits total Pods/Services.<br>**Stability:** Prevents "Noisy Neighbor" syndrome. |
| **03.lr.yml** | **Limit Range** | **Defaulting:** Assigns CPU/RAM if developer forgets.<br>**Constraints:** Enforces Min/Max pod sizes.<br>**Granularity:** Works at the container level. |

---

## 📦 Phase 2: Workloads & Scaling

### 📄 04.po.yml — Pod
* **Smallest Unit:** The basic building block of Kubernetes that runs your container.
* **Ephemeral:** Pods are designed to die; they are not permanent (use Deployments for permanence).
* **Shared Resources:** Containers inside the same Pod share the same network (IP) and storage.

### 📄 05.rc.yml — Replication Controller
* **Legacy Tool:** This is the older version of the ReplicaSet.
* **Desired State:** It ensures that exactly "X" number of Pods are running at all times.
* **Self-Healing:** If a Pod crashes, the RC detects it and starts a new one.

### 📄 06.rs.yml — ReplicaSet
* **Modern Scaling:** The successor to the RC with better "Label Selectors."
* **Grouping:** It uses complex logic to decide which Pods it should manage.
* **Deployment Component:** Usually, you don't create these manually; the Deployment creates them for you.

### 📄 07.dep.yml — Deployment
* **Management:** The most common way to deploy `vgdorder.online`.
* **Rolling Updates:** It allows you to update your app with **zero downtime**.
* **Rollbacks:** If a new version fails, you can quickly revert to a previous stable version.

---

## 🌐 Phase 3: Networking & Advanced Workloads

### 📄 08.svc.yml — Service
* **Stable IP:** Pods change IPs often; the Service provides a single, permanent IP to access them.
* **Load Balancing:** It distributes incoming traffic across all healthy Pods.
* **External Access:** Using `type: LoadBalancer` creates a cloud-native entry point (e.g., AWS ELB).

### 📄 09.hpa.yml — Horizontal Pod Autoscaler
* **Elasticity:** Automatically adds more Pods when CPU usage spikes (e.g., during a sale).
* **Cost Saving:** Removes Pods when traffic is low to save money on compute instances.
* **Metrics-Based:** Relies on the metrics-server installed in the cluster.

### 📄 10.ds.yml — DaemonSet
* **One-Per-Node:** Ensures exactly one copy of a Pod runs on every worker node.
* **Infrastructure Tasks:** Perfect for log collectors (Fluentd) or monitoring agents (Datadog).
* **Automatic Deployment:** If you add a new node, the DS automatically starts a Pod there.

---

## 💾 Phase 4: Persistence & Configuration

### 🗄️ Storage Logic
* **11.pv.yml (Persistent Volume):** The actual physical disk. It is a **Global** resource.
* **12.pvc.yml (Persistent Volume Claim):** The "ticket" a Pod uses to ask for storage. **Namespace-bound**.

### ⚙️ Configuration Logic
* **13.cm.yml (ConfigMap):** Keeps settings (URLs, Ports) separate from code.
* **14.sec.yml (Secret):** Used for sensitive data (Passwords, Keys). Data is Base64 encoded and stored in RAM (`tmpfs`) for security.

---

## 🛡️ Phase 5: RBAC (Role-Based Access Control)

| Type | Resource | Scope | Description |
| :--- | :--- | :--- | :--- |
| **Local** | **15.ro.yml (Role)** | Namespace | Defines specific actions (get, list, delete) allowed in one area. |
| **Local** | **16.rob.yml (RoleBinding)** | Namespace | Links a Role to a User (e.g., Jagadish) to grant local access. |
| **Global** | **17.cr.yml (ClusterRole)** | Cluster | Permissions for global resources like Nodes or PVs. |
| **Global** | **18.crb.yml (ClusterRoleBinding)** | Cluster | Grants Admin-level power across the **entire cluster**. |

---

> 💡 **Interview Tip:** Mention that you follow the **Principle of Least Privilege** by using Roles instead of ClusterRoles wherever possible to keep the cluster secure.
