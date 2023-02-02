---
title: "Hashicorp Vault"
date: 2023-02-02T04:12:01Z
weight: 5
---

Vault, by HashiCorp, is a purpose built, open source tool for storing and managing
Secrets in a secure manner.


## Installation and Setup

### Helm chart
The easiest way to install Vault is using the official Helm chart maintained by HashiCorp.

```
helm repo add hashicorp https://helm.releases.hashicorp.com

helm install vault hashicorp/vault \
  --set server.dev.enabled=true \
```

### Vault CLI

We can download and install Vault CLI from [the official website](https://developer.hashicorp.com/vault/downloads)

Once installed, Vault can be accessed through standard port forwarding, and visiting
the UI at http://localhost:8200

### Usage

Once Vault is installed in your cluster, it’s time to store your first Secret in Vault:

```
vault kv put secret/hello foo=world
```

Retrieve secret:

```
vault kv get secret/hello
```

## Vault Agent Sidecar injector

For an application to retrieve Secrets from Vault, we need to perform some prerequisite steps before launching the application. We need a security token to access secrets in k8s. That would take us back to chicken-and-egg problem. To solve this problem, HashiCorp developed the Vault Agent Sidecar Injector.

### How it works

The Vault Agent Sidecar Injector automatically modifies Pods that are annotated a specific way, and securely retrieves annotated
Secret references (application Secrets) and renders those values into a shared volume
accessible to the application container.

### Installation

We pass an additional argument ```injector.enabled=true``` to helm chart installation:

```
 helm install vault hashicorp/vault \
 --set server.dev.enabled=true \
 --set injector.enabled=true
```

### Usage

When an application desires to retrieve its Secrets from Vault, the Pod spec needs to have at a minimum the following Vault agent annotations.  annotations:
```
vault.hashicorp.com/agent-inject: "true"
vault.hashicorp.com/agent-inject-secret-hello.txt: secret/hello
vault.hashicorp.com/role: app
```

These annotations convey several pieces of information:
  * The annotation key vault.hashicorp.com/agent-inject: "true" informs the Vault Agent Sidecar Injector that Vault Secret injection should
occur for this Pod.
  * The annotation value secret/hello indicates which Vault Secret key to inject into the Pod.
  * The suffix hello.txt of the annotation, vault.hashicorp.com/agentinject-secret-hello.txt, indicates that the Secret should be populated under a file named hello.txt in the shared memory volume with the final path being /vault/secrets/hello.txt.
  * The annotation value from the vault.hashicorp.com/role indicates which
Vault role should be used when retrieving the Secret.

### Configuration the sidecar injector access to secrets

We need to configure Vault to allow Kubernetes Pods to authenticate and retrieve Secrets. To do so, run the following Vault commands to enable the Kubernetes auth method:

```
vault auth enable kubernetes
Success! Enabled kubernetes auth method at: kubernetes/

vault write auth/kubernetes/config \
 token_reviewer_jwt="$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" \
 kubernetes_host="https://$KUBERNETES_PORT_443_TCP_ADDR:443" \
 kubernetes_ca_cert=@/var/run/secrets/kubernetes.io/serviceaccount/ca.crt

Success! Data written to: auth/kubernetes/config
```

These two commands configure Vault to use the Kubernetes authentication method to use the service account token, the location of the Kubernetes host, and its certificate.

Next, we define a policy named “app,” as well as a role named “app,” which will
have read privileges to the “hello” Secret:

```
# Create a policy "app" which will have read privileges to the "secret/hello" secret
$ vault policy write app - <<EOF
path "secret/hello" {
 capabilities = ["read"]
}
EOF
# Grants a pod in the "default" namespace using the "default" service account
# privileges to read the "hello" secret
$ vault write auth/kubernetes/role/app \
 bound_service_account_names=default \
 bound_service_account_namespaces=default \
 policies=app \
 ttl=24h
```