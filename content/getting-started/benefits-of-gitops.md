---
title: "Benefits of Gitops"
date: 2023-01-11T09:35:32+07:00
weight: 6
---

## Benefits of GitOps

Combining a GitOps methodology with Kubernetes’ declarative configuration and
active reconciliation model provides many operational benefits that provide a more
predictable and reliable system.

### Declarative

In an imperative model, we specify *a series of steps to instruct the system* how to reach our desired state. In contrast, with declarative models, we describe **what we want to achieve as opposed to how to get there**.

GitOps model solely uses declarative manifests stored in Git repo that declare the desired state and let a gitops tool to detect and converge with kubernetes cluster.

### Disaster recovery

GitOps helps in the recovery of infrastructure environments by storing declarative
specifications of the environment under source control as a source of truth. Having a
complete definition of what the environment should be facilitates the re-creation of
the environment in the event of a disaster. **Disaster recovery becomes a simple exer-
cise of (re)applying all the configuration stored in the Git repository**.

### Observability

Observability mechanisms can help answer the question, “What’s cur-
rently running in my environment?”

Deployed environments are expected
to be observable. In other words, you should always be able to inspect an environment
to see what is currently running and how things are configured. If the environment’s running state can be
observed, and the desired state of the environment is
detect divergence from the
defined in Git, the environment can be verified by
desired state.




