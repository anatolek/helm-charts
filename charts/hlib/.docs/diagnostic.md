### `hlib.diagnostic` template

Creates multiple Kubernetes resources that help developers provide extended access to the Kubernetes cluster for debugging.

#### Providing access

Generate a kubeconfig with admin access and send it to the engineer who needs this access

```shell
# Get current context
CLUSTER_NAME=$(kubectl config view --minify -o jsonpath='{.clusters[0].name}' | cut -d'/' -f2)
CLUSTER_SERVER=$(kubectl config view --minify -o jsonpath='{.clusters[0].cluster.server}')
CLUSTER_CA=$(kubectl config view --raw --minify -o jsonpath='{.clusters[0].cluster.certificate-authority-data}')
CLUSTER_NAMESPACE=<NAMESPACE_NAME>
TOKEN=$(kubectl get secret <SECRET_NAME> -o jsonpath='{.data.token}' | base64 -d)


cat <<EOF > debug-kubeconfig.yaml
apiVersion: v1
kind: Config
clusters:
- name: ${CLUSTER_NAME}
  cluster:
    server: ${CLUSTER_SERVER}
    certificate-authority-data: ${CLUSTER_CA}
contexts:
- name: ${CLUSTER_NAME}-debug
  context:
    cluster: ${CLUSTER_NAME}
    user: debug
    namespace: ${CLUSTER_NAMESPACE}
current-context: ${CLUSTER_NAME}-debug
users:
- name: debug
  user:
    token: ${TOKEN}
EOF
```

Kubeconfig usage

```shell
export KUBECONFIG=debug-kubeconfig.yaml
kubectl ...
```

> NOTE: This instruction may also appear when deploying an application with debug mode enabled.

#### Default access permissions

Namespace-scoped Role Permissions

| API Group    | Resources                                                  | Verbs         | Purpose                                      |
|--------------|------------------------------------------------------------|---------------|----------------------------------------------|
| `""`         | `pods`, `pods/log`                                         | `get`, `list` | View pod details and logs                    |
| `""`         | `pods/portforward`                                         | `create`      | Port-forward into pods                       |
| `""`         | `events`, `configmaps`                                     | `get`, `list` | View events and ConfigMaps                   |
| `apps`       | `replicasets`, `deployments`, `statefulsets`, `daemonsets` | `get`, `list` | Debug controllers managing workloads         |

Cluster-scoped ClusterRole Permissions

| API Group        | Resources                                                  | Verbs         | Purpose                                      |
|------------------|------------------------------------------------------------|---------------|----------------------------------------------|
| `""`             | `namespaces`                                               | `get`, `list` | Discover available namespaces                |
| `""`             | `nodes`, `persistentvolumes`                               | `get`, `list` | Understand node states and volume bindings   |
| `metrics.k8s.io` | `pods`                                                     | `get`, `list` | View pod-level resource usage metrics        |

Additional permissions can be configured using the following variables:

- `.Values.diagnosticMode.role.extraPermissionRules`
- `.Values.diagnosticMode.clusterRole.extraPermissionRules`
