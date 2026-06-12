# Kubernetes Storage — Hands-on Assignment

## Topic: Mounting ConfigMaps and Secrets as Volumes

**Difficulty:** Intermediate  
**Estimated Time:** 20–30 minutes  
**Namespace:** `config-lab`

---

## Problem Statement

An application pod requires access to a configuration file and a database credential at runtime. Rather than hardcoding these values, your task is to supply them via a ConfigMap and a Secret, both mounted as files inside the same pod.

---

### Task 1 — Create the Namespace
Create a namespace called `config-lab`.

---

### Task 2 — Create a ConfigMap
Create a ConfigMap named `app-config` in the `config-lab` namespace containing a single key called `app.conf` with the following content:

```
env=production
log_level=info
max_connections=50
timeout=30s
```

---

### Task 3 — Create a Secret
Create a Secret named `db-secret` in the `config-lab` namespace containing a single key called `db-password` with the value:

```
S3cur3P@ssw0rd!
```

---

### Task 4 — Deploy a Pod with Both Volumes Mounted
Create a Pod in the `config-lab` namespace with the following specifications:

| Field | Value |
|---|---|
| Name | `config-pod` |
| Image | `busybox:1.36` |
| Command | `sleep 3600` |

Mount the volumes as follows:

| Volume | Type | Mount Path in Container |
|---|---|---|
| `app-config` ConfigMap | configMap | `/etc/app-config` |
| `db-secret` Secret | secret | `/etc/db-secret` |

---

### Task 5 — Verify File Contents
Exec into the pod and confirm:

1. The file `/etc/app-config/app.conf` exists and contains the correct configuration content.
2. The file `/etc/db-secret/db-password` exists and contains the correct password value.

---

### Task 6 — Verify Secret is on tmpfs
Exec into the pod and run `df /etc/db-secret` to inspect the filesystem type of the Secret mount.

Confirm that it is mounted on `tmpfs` and not on the node's disk. Note why Kubernetes does this for Secrets.

---

### Bonus Challenge
Update the ConfigMap's `log_level` value from `info` to `debug` without recreating the pod. Wait a moment, then exec into the pod again and check whether `/etc/app-config/app.conf` reflects the updated value automatically. Does the Secret update the same way?