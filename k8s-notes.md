1. 01.ns.yml (Namespace)
Isolation: It creates a "virtual cluster" so your vgdorder resources don't mix with other projects.

Security: You can apply permissions (RBAC) specifically to this namespace.

Resource Management: It allows you to set quotas so one app doesn't crash the whole cluster.

2. 02.rq.yml (Resource Quota)
Hard Limits: Sets a maximum amount of CPU and Memory the entire namespace can use.

Object Counting: Limits the total number of Pods or Services that can be created.

Stability: Prevents "noisy neighbor" syndrome where one app steals all resources from others.

3. 03.lr.yml (Limit Range)
Default Values: If a developer forgets to define CPU/Memory in their Pod, this file automatically assigns them.

Min/Max Constraints: Forces Pods to stay within a specific size (e.g., no Pod smaller than 100m CPU).

Granular Control: Works at the container level rather than the whole namespace level.

4. 04.po.yml (Pod)
Smallest Unit: The basic building block of Kubernetes that runs your container.

Ephemeral: Pods are designed to die; they are not permanent (use Deployments for permanence).

Shared Resources: Containers inside the same Pod share the same network (IP) and storage.

5. 05.rc.yml (Replication Controller)
Legacy Tool: This is the older version of the ReplicaSet.

Desired State: It ensures that exactly "X" number of Pods are running at all times.

Self-Healing: If a Pod crashes, the RC detects it and starts a new one.

6. 06.rs.yml (ReplicaSet)
Modern Scaling: The successor to the RC with better "Label Selectors."

Grouping: It uses complex logic to decide which Pods it should manage.

Deployment Component: Usually, you don't create these manually; the Deployment creates them for you.

7. 07.dep.yml (Deployment)
Management: The most common way to deploy vgdorder.online.

Rolling Updates: It allows you to update your app with zero downtime.

Rollbacks: If a new version fails, you can quickly revert to a previous stable version.

8. 08.svc.yml (Service)
Stable IP: Pods change IPs often; the Service provides a single, permanent IP to access them.

Load Balancing: It distributes incoming traffic across all healthy Pods.

External Access: Using type: LoadBalancer creates an AWS ELB so the world can see your site.

9. 09.hpa.yml (Horizontal Pod Autoscaler)
Elasticity: Automatically adds more Pods when CPU usage spikes (e.g., during a sale on vgdorder.online).

Cost Saving: Removes Pods when traffic is low to save money on EC2 instances.

Metrics-Based: Relies on the metrics-server you installed during EKS setup.

10. 10.ds.yml (DaemonSet)
One-Per-Node: Ensures that exactly one copy of a Pod runs on every worker node.

Infrastructure Tasks: Perfect for log collectors (Fluentd) or monitoring agents (Datadog).

Automatic Deployment: If you add a new node to your AWS cluster, the DS automatically starts a Pod there.

11. 11.pv.yml (Persistent Volume)
Actual Storage: Represents the physical disk (EBS volume) in the AWS cloud.

Cluster-Wide: It exists outside of any namespace; it is a global resource.

Lifecycle: The data stays safe even if the Pod using it is deleted.

12. 12.pvc.yml (Persistent Volume Claim)
Request Form: Think of this as a "ticket" that a Pod uses to ask for storage.

Binding: K8s looks for a PV that matches the PVC's size and connects them.

Namespace-Bound: Unlike the PV, the claim lives inside the vgd-prod namespace.

13. 13.cm.yml (ConfigMap)
External Config: Keeps your code separate from your settings (URLs, Port numbers).

Environment Variables: Injects settings into your container without rebuilding the image.

Dynamic Updates: If you change the ConfigMap, many apps can see the change without restarting.

14. 14.sec.yml (Secret)
Sensitive Data: Used for passwords, SSH keys, and API tokens.

Encoding: Data is Base64 encoded (though not encrypted by default, it's safer than a ConfigMap).

Memory-Mounted: Secrets are usually stored in tmpfs (RAM) on the node for better security.

15. 15.ro.yml (Role)
Local Permissions: Defines what can be done (get, list, delete) within one namespace.

Resource Specific: You can say "this user can only look at Pods, but not touch Secrets."

Safety: Follows the "Principle of Least Privilege."

16. 16.rob.yml (RoleBinding)
The Link: Connects a Role (permissions) to a User (like jagadish).

Namespace Scope: Only gives the user permissions inside that specific namespace.

Authorization: This is why you get "Forbidden" errors if this file isn't set up correctly.

17. 17.cr.yml (ClusterRole)
Global Permissions: Like a Role, but works across the entire cluster.

Non-Namespaced Resources: Used to give permission to see Nodes or PersistentVolumes.

Reusability: You can define a ClusterRole once and bind it to users in multiple namespaces.

18. 18.crb.yml (ClusterRoleBinding)
Global Access: Gives a user admin-like powers across every single namespace.

High Power: Use this carefully! Only give it to users who truly need to manage the whole cluster.

Critical for jagadish: Since you had "Forbidden" errors earlier, this is the file that would have fixed it by making you a cluster-wide admin.
