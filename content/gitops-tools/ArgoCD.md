---
title: "ArgoCD"
date: 2023-01-05T10:59:48+07:00
---

One of the most popular tools to deploy resources to k8s cluster.

## ArgoCD Application Dependencies

### App of Apps pattern
The App of Apps pattern design is basically an Argo CD application made up of other argoCD applications. The purpose is to bootstrapping our application and its dependencies. Administrators need a way to deploy argoCD applications using argoCD itself. The solution is to create an application made up of other argoCD applications.

For example, we have an application that has dependencies on: 
- Cert-Manager
- Backend Application 
- Ingress
- Frontend Application
- etc...

Instead of deploying a bunch of argoCD applications individually, we could deploy one argoCD application that deploys other application.

This is very convenient because it provides an entry point and bootstraps dependencies before the main application goes online.

### SyncWaves and Resource Hooks

SyncWaves and resource hooks are a way we can order how argoCD applies individual manifests to allocation resources on k8s cluster. We use annotation (not labels) to annotate the object with the order we'd like to apply the manifest, number based with lowest going first. For example, we have a `Deployment` numbered as 0 and a `Service` as 1, argoCD will apply the `Deployment` first, waiting for it to report healthy, then apply the `Service`



## test 2
### test 3
#### test 4

