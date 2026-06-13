# Kubernetes Storage — Hands-on Assignment

## Topic: Persistent Volumes, Persistent Volume Claims & Pod Volume Mounts

**Difficulty:** Intermediate  
**Estimated Time:** 10–15 minutes  
**Namespace:** `ns01`

---

## Problem Statement

A development team requires persistent storage for a lightweight utility pod. Your task is to manually provision the storage layer and attach it to a running pod.

---

### Task 1 — Create the Namespace
Create a namespace called `ns01`. All resources must be created in this namespace unless stated otherwise.  
*(Note: PersistentVolumes are cluster-scoped.)*

---

### Task 2 — Create a PersistentVolume
Create a PersistentVolume with the following specifications:

| Field | Value |
|---|---|
| Name | `lab-pv` |
| Capacity | `200Mi` |
| Access Mode | `ReadWriteOnce` |
| Reclaim Policy | `Retain` |
| Storage Class | `manual` |
| Host Path | `/mnt/lab-data` |

---

### Task 3 — Create a PersistentVolumeClaim
Create a PersistentVolumeClaim in the `ns01` namespace that binds to the PV created above:

| Field | Value |
|---|---|
| Name | `lab-pvc` |
| Namespace | `ns01` |
| Requested Storage | `200Mi` |
| Access Mode | `ReadWriteOnce` |
| Storage Class | `manual` |

---

### Task 4 — Deploy a Pod using the PVC
Create a Pod in the `ns01` namespace with the following specifications:

| Field | Value |
|---|---|
| Name | `lab-pod` |
| Image | `busybox:1.36` |
| Command | `sleep 3600` |
| Volume Name | `lab-storage` |
| Mount Path (in container) | `/data` |
| PVC | `lab-pvc` |

---

### Task 5 — Validate Persistence
Once the pod is running, write the string `storage-lab-test` to a file called `test.txt` inside `/data` in the container, then verify you can read it back.

---

### Bonus Challenge
Delete the pod and recreate it using the same PVC. Verify that `/data/test.txt` still exists with its original content.