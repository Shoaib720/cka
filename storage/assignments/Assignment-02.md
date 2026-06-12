# Kubernetes Storage — Hands-on Assignment

## Topic: StorageClass, Dynamic Provisioning & Pod Volume Mounts

**Difficulty:** Intermediate  
**Estimated Time:** 20–30 minutes  
**Namespace:** `dynamic-storage-lab`

---

## Problem Statement

A development team wants to move away from manually provisioned storage. Your task is to set up a StorageClass backed by a local provisioner, allow Kubernetes to dynamically provision a PersistentVolume, and attach the storage to a running pod.

> Assume `rancher.io/local-path` provisioner is available on your cluster. If not, install it before proceeding:  
> `kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml`

---

### Task 1 — Create the Namespace
Create a namespace called `dynamic-storage-lab`.

---

### Task 2 — Create a StorageClass
Create a StorageClass with the following specifications:

| Field | Value |
|---|---|
| Name | `local-dynamic` |
| Provisioner | `rancher.io/local-path` |
| Reclaim Policy | `Delete` |
| Volume Binding Mode | `WaitForFirstConsumer` |

---

### Task 3 — Create a PersistentVolumeClaim
Create a PersistentVolumeClaim in the `dynamic-storage-lab` namespace referencing the StorageClass above:

| Field | Value |
|---|---|
| Name | `dynamic-pvc` |
| Namespace | `dynamic-storage-lab` |
| Requested Storage | `100Mi` |
| Access Mode | `ReadWriteOnce` |
| Storage Class | `local-dynamic` |

> **Note:** Observe the PVC status after creation. Does it bind immediately? Why or why not?

---

### Task 4 — Deploy a Pod using the PVC
Create a Pod in the `dynamic-storage-lab` namespace with the following specifications:

| Field | Value |
|---|---|
| Name | `dynamic-pod` |
| Image | `busybox:1.36` |
| Command | `sleep 3600` |
| Volume Name | `dynamic-storage` |
| Mount Path (in container) | `/data` |
| PVC | `dynamic-pvc` |

> **Note:** After the pod is scheduled, check the PVC status again. What changed and why?

---

### Task 5 — Validate Dynamic Provisioning
Once the pod is running, confirm that a PersistentVolume was automatically created by the provisioner without you defining one manually.

Then write the string `dynamic-test` to a file called `output.txt` inside `/data` in the container and verify you can read it back.

---

### Bonus Challenge
Delete the pod and the PVC. Check whether the dynamically provisioned PV still exists. Explain the behaviour based on the reclaim policy set on the StorageClass.