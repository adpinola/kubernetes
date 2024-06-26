# Kubernetes Samples

The [management-cluster](./management-cluster/) folders contain kubernetes manifests to be deployed in the cluster where Flux is running. The Kustomization referencing this deployment is located at [./cd/clusters/management](./cd/clusters/management/)

The [target-demo](./target-demo/) folder contains kubernetes manifests to be applied in a different cluster. The Kustomization referencing this deployment is located at [./cd/clusters/target-demo](./cd/clusters/target-demo/)

The flux-system related deployments are located in the [./cd/flux-system](./cd/flux-system) folder.

Both HelmRelease and Kustomization in charge of deploying Strimzi and Kafka in the target-demo cluster have the `kubeConfig` property configure with a reference to a secret containing cluster credentials. This must be configured in remote cluster deployments.

```YAML
kubeConfig:
  secretRef:
    name: target-demo-kubeconfig
```

The `target-demo-kubeconfig` secret is handled by the External Secrets operator, configured [here](./management-cluster/external-secrets.yaml)

The Kubernetes secret is reconciled based on a Secret stored in an Azure Key Vault. To allow access to the Key Vault, Workload Identities were configured with the proper RBAC configuration. See [this document](https://learn.microsoft.com/en-us/azure/aks/workload-identity-deploy-cluster#update-an-existing-aks-cluster) for reference.

The Secret in KeyVault contains a JSON with the following properties:

```json
{
	"clusterName": "",
	"serverUrl": "",
	"certData": "",
	"clientKey": "",
	"caData": "",
	"token": ""
}
```