### `hlib.role` template

Creates Kubernetes Role resources.

#### Basic Usage

Include this template in your chart's `templates/role.yaml`:

```handlebars
{{- include "hlib.role" (dict "context" .) }}
```

#### Configuration

| Parameter  | Description                                                               | Required | Default             |
|------------|---------------------------------------------------------------------------|----------|---------------------|
| `context`  | Root Helm context (usually `.`)                                           | Yes      | -                   |
| `values`   | Role configuration values (from `values.yaml`)                            | No       | `.Values.rbac.role` |
| `override` | Name of a template that overrides the basic one configured in the library | No       | -                   |

#### Advanced: Template Overrides

The changes can only be added to the base template via `override` parameter, e.g.:

```handlebars
{{- include "hlib.role" (dict "context" . "override" "app.role") -}}

{{- define "app.role" -}}
rules:
  - apiGroups: [""]
    resources: ["configmaps", "pods"]
    verbs: ["get", "list"]
{{- end -}}
```
