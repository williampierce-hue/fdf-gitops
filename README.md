# fdf-gitops

GitOps source of truth for the Dominion FDF demo, hosted on **GitHub**. One ArgoCD
on the hub (OpenShift GitOps) syncs each directory below to the matching cluster,
registered through RHACM/ACM.

```
clusters/
  local-cluster/   -> the hub / Primary (ACM name: local-cluster)
  aro/             -> Azure Red Hat OpenShift
  rosa/            -> ROSA on AWS
```

Each directory holds a `kustomization.yaml` and the resources for that cluster.
The demo payload is an ArgoCD-managed ConfigMap (`gitops-managed` in namespace
`gitops-demo`): edit it and commit to see ArgoCD reconcile all clusters; delete it
in a cluster's console to watch ArgoCD self-heal it from Git.

A **public** repo is recommended — it contains no secrets, ArgoCD pulls it with no
credential, and reviewers (e.g. the IBM team) can read it with no account.

Setup steps are in `GitOps_Setup.docx`.
