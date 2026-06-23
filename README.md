# fdf-gitops

GitOps source of truth for the Dominion **IBM Fusion Data Foundation** demo, hosted
on GitHub. One ArgoCD on the hub (OpenShift GitOps) syncs each directory below to the
matching cluster, registered through RHACM/ACM.

```
clusters/
  local-cluster/   -> the hub / Primary (ACM name: local-cluster)
  aro/             -> Azure Red Hat OpenShift
  rosa/            -> ROSA on AWS
```

Each directory deploys the **same three FDF (Ceph RBD) storage resources** —
satisfying Scenario 5 ("deploy storage resources via ArgoCD from a Git repository"):

| Resource | Name | What it is |
|----------|------|------------|
| `StorageClass` | `gitops-ceph-rbd` | provisions from the Fusion Data Foundation Ceph RBD pool |
| `PersistentVolumeClaim` | `gitops-demo-pvc` (5Gi) | a real volume Bound on FDF storage |
| `VolumeSnapshotClass` | `gitops-rbd-snapclass` | the snapshot policy for FDF RBD |

Sync-wave annotations create the StorageClass before the PVC that consumes it.

**Demo:** edit a manifest and commit → ArgoCD reconciles it on every cluster; delete
the StorageClass (or PVC) in a cluster's console → ArgoCD recreates it from Git.
One commit, three infrastructures, all on IBM Fusion Data Foundation.

A **public** repo is recommended — it contains no secrets, ArgoCD pulls it with no
credential, and reviewers (e.g. the IBM team) can read it with no account.

Setup steps are in `GitOps_Setup.docx`.
