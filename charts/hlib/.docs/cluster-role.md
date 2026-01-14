### `hlib.clusterRole` template

Creates Kubernetes ClusterRole resources.

#### Basic Usage

Include this template in your chart's `templates/cluster-role.yaml`:

```handlebars
{{- include "hlib.clusterRole" (dict "context" .) }}
```

#### Configuration

| Parameter      | Description                                                               | Required | Default                    |
|----------------|---------------------------------------------------------------------------|----------|----------------------------|
| `context`      | Root Helm context (usually `.`)                                           | Yes      | -                          |
| `values`       | ClusterRole configuration values (from `values.yaml`)                     | No       | `.Values.rbac.clusterRole` |
| `override`     | Name of a template that overrides the basic one configured in the library | No       | -                          |

**Role Aggregation**

Combine existing ClusterRoles using label selectors, e.g.

```yaml
rbac:
  clusterRole:
    aggregationClusterRoleSelectors:
      - matchLabels:
          rbac.group/type: "monitoring"
```

#### Advanced: Template Overrides

The changes can only be added to the base template via `override` parameter, e.g.:

```handlebars
{{- include "hlib.clusterRole" (dict "context" . "override" "app.clusterRole") -}}

{{- define "app.clusterRole" -}}
rules:
  - apiGroups: [""]
    resources: ["endpoints", "pods", "nodes", "services"]
    verbs: ["get", "list"]
{{- end -}}
```
