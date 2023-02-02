+++
title = "Gitops Secret Management"
date = 2023-02-02T04:08:27Z
weight = 7
chapter = true
pre = "<b>3. </b>"
+++

### Chapter 3

# Secret Management for ArgoCD

base64 encoding is **not** encryption. Secrets in k8s is not encrypted but encoded. This is why we need to implement some other methods to encrypt secrets.