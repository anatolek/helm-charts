### `hlib.clusterRoleBinding` template

Creates Kubernetes ClusterRoleBinding resources.
By default, only one binding is created per service account.

#### Basic Usage

Include this template in your chart's `templates/cluster-role-binding.yaml`:

```handlebars
{{- include "hlib.clusterRoleBinding" (dict "context" .) }}
```

#### Configuration

| Parameter      | Description                                                               | Required | Default                           |
|----------------|---------------------------------------------------------------------------|----------|-----------------------------------|
| `context`      | Root Helm context (usually `.`)                                           | Yes      | -                                 |
| `values`       | ClusterRoleBinding configuration values (from `values.yaml`)              | No       | `.Values.rbac.clusterRoleBinding` |
| `dependencies` | Dictionary of dependent components (see below)                            | No       | -                                 |
| `override`     | Name of a template that overrides the basic one configured in the library | No       | -                                 |

**Dependency Integration**

Used to override the configuration path of resources on which it depends.

| Dependency       | Usage                     | Default                    |
|------------------|---------------------------|----------------------------|
| `clusterRole`    | Cluster role reference    | `.Values.rbac.clusterRole` |
| `serviceAccount` | Service account reference | `.Values.serviceAccount`   |

```handlebars
{{- $dependencies := dict "clusterRole" .Values.newClusterRole "serviceAccount" .Values.newServiceAccount -}}
{{- include "hlib.deployment" (dict "context" . "dependencies" $dependencies) }}
```

#### Advanced: Template Overrides

The changes can only be added to the base template via `override` parameter, e.g.:

```handlebars
{{- include "hlib.clusterRoleBinding" (dict "context" . "override" "app.clusterRoleBinding") -}}

{{- define "app.clusterRoleBinding" -}}
metadata:
  name: {{ printf "%s-rb" (include "hlib.name" $) | trunc 63 | trimSuffix "-" }}
{{- end -}}
```
