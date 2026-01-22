To move from a beginner to a DevOps professional, you need to know commands that help you **debug**, **audit**, and **manage** Pods under pressure.

Here is the "DevOps Survival Kit" for Pod commands:

### 1. The "What’s Wrong?" Commands (Troubleshooting)

When a Pod is crashing or stuck, these are your first line of defense.

* **`kubectl describe pod <name>`**: This is the most important command. It shows "Events" at the bottom (e.g., *Failed to pull image*, *Out of Memory*, *Liveness probe failed*).
* **`kubectl logs <name> --previous`**: Essential for **CrashLoopBackOff**. It shows the logs of the container *before* it crashed, which is usually where the error message is hidden.
* **`kubectl get pods -o wide`**: Shows which **Node** the Pod is on and its internal **IP address**. Great for checking if a specific node is causing all the failures.

### 2. The "Deep Dive" Commands (Inspection)

Sometimes you need to see exactly what Kubernetes "thinks" your Pod looks like.

* **`kubectl get pod <name> -o yaml`**: This shows the full, live configuration. DevOps engineers use this to check if Environment Variables or Secret mounts were actually applied correctly.
* **`kubectl exec -it <name> -- env`**: Instead of entering the shell, this quickly lists all environment variables inside the container.
* **`kubectl top pod`**: Shows real-time **CPU and Memory** usage. (Note: This requires the *Metrics Server* to be installed in your cluster).

### 3. The "Power User" Flags

These flags save you time when managing dozens of Pods.

* **`-A` (All Namespaces):** `kubectl get pods -A` lets you see everything in the cluster at once, instead of just the "default" area.
* **`-l` (Labels):** `kubectl get pods -l app=nginx` filters Pods by their labels. In production, you never look for one Pod; you look for a group of them.
* **`--watch` or `-w**`: `kubectl get pods -w` keeps the terminal open and updates instantly when a Pod status changes from `Pending` to `Running`.

### 4. The "Quick Fix" Commands

* **`kubectl delete pod <name> --grace-period=0 --force`**: The "Nuclear Option." Use this only if a Pod is stuck in "Terminating" and won't go away.
* **`kubectl rollout restart deployment <deployment-name>`**: The professional way to "restart" Pods. Since you shouldn't manage Pods individually, this tells the Controller to replace all of them safely.

---

### Summary Checklist

| If you want to... | Use this command... |
| --- | --- |
| Find out why a Pod won't start | `kubectl describe pod` |
| See the error after a crash | `kubectl logs --previous` |
| Check live resource usage | `kubectl top pod` |
| Update the Pod's image/code | `kubectl rollout restart` |

