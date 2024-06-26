# Secret Management

Argo CD is un-opinionated about how secrets are managed. There are many ways to do it, and there's no one-size-fits-all solution.

Many solutions use plugins to inject secrets into the application manifests. See [Mitigating Risks of Secret-Injection Plugins](#mitigating-risks-of-secret-injection-plugins)
below to make sure you use those plugins securely.

Here are some ways people are doing GitOps secrets:

* [Bitnami Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets)
* [External Secrets Operator](https://github.com/external-secrets/external-secrets)
* [Hashicorp Vault](https://www.vaultproject.io)
* [Bank-Vaults](https://bank-vaults.dev/)
* [Helm Secrets](https://github.com/jkroepke/helm-secrets)
* [Kustomize secret generator plugins](https://github.com/kubernetes-sigs/kustomize/blob/fd7a353df6cece4629b8e8ad56b71e30636f38fc/examples/kvSourceGoPlugin.md#secret-values-from-anywhere)
* [aws-secret-operator](https://github.com/mumoshu/aws-secret-operator)
* [KSOPS](https://github.com/viaduct-ai/kustomize-sops#argo-cd-integration)
* [argocd-vault-plugin](https://github.com/argoproj-labs/argocd-vault-plugin)
* [argocd-vault-replacer](https://github.com/crumbhole/argocd-vault-replacer)
* [Kubernetes Secrets Store CSI Driver](https://github.com/kubernetes-sigs/secrets-store-csi-driver)
* [Vals-Operator](https://github.com/digitalis-io/vals-operator)
* [argocd-secret-replacer](https://github.com/mmalyska/argocd-secret-replacer)

For discussion, see [#1364](https://github.com/argoproj/argo-cd/issues/1364)

## Mitigating Risks of Secret-Injection Plugins

Argo CD caches the manifests generated by plugins, along with the injected secrets, in its Redis instance. Those
manifests are also available via the repo-server API (a gRPC service). This means that the secrets are available to
anyone who has access to the Redis instance or to the repo-server.

Consider these steps to mitigate the risks of secret-injection plugins:

1. Set up network policies to prevent direct access to Argo CD components (Redis and the repo-server). Make sure your
   cluster supports those network policies and can actually enforce them.
2. Consider running Argo CD on its own cluster, with no other applications running on it.

