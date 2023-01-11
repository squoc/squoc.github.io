---
title: "What Is Gitops?"
date: 2023-01-05T10:46:51+07:00
weight: 5
---

## What is GitOps?

GitOps is a set of best practices for declaratively observing and describing a system’s operating infrastructure. You can apply the GitOps methodology throughout the application development workflow, **using Git as a single source of truth** to actively reconcile and declaratively configure an application. 

In a GitOps model, the system’s desired configuration is *stored in a revision con-
trol system, such as Git*. Instead of making changes directly to the system via a UI or
CLI, an engineer makes changes to the configuration files that represent the desired
state. A difference between the desired state stored in Git and the system’s actual state
indicates that not all changes have been deployed. These changes can be reviewed
and approved through standard revision control processes such as pull requests, code
reviews, and merges to master.

When changes have been approved and merged to the
main branch, an operator software process is responsible for changing the system’s
current state to the desired state based on the configuration stored in Git.

GitOps doesn’t require a particular set of tools, but the tools must offer this stan-
dard functionality:
- Operate on the desired state of the system that is stored in Git
- Detect differences between the desired state and the actual state
- Perform the required operations on the infrastructure to synchronize the
actual state with the desired state
Although this book focuses on GitOps in relation to Kubernetes, many of the princi-
ples of GitOps could be implemented independently of Kubernetes.

