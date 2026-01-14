### `hlib.roleBinding` template

Creates Kubernetes roleBinding resources.
By default, only one binding is created per service account.

#### Basic Usage

Include this template in your chart's `templates/role-binding.yaml`:

```handlebars
{{- include "hlib.roleBinding" (dict "context" .) }}
```

#### Configuration

| Parameter      | Description                                                               | Required | Default                    |
|----------------|---------------------------------------------------------------------------|----------|----------------------------|
| `context`      | Root Helm context (usually `.`)                                           | Yes      | -                          |
| `values`       | roleBinding configuration values (from `values.yaml`)                     | No       | `.Values.rbac.roleBinding` |
| `dependencies` | Dictionary of dependent components (see below)                            | No       | -                          |
| `override`     | Name of a template that overrides the basic one configured in the library | No       | -                          |

**Dependency Integration**

Used to override the configuration path of resources on which it depends.

| Dependency       | Usage                     | Default                  |
|------------------|---------------------------|--------------------------|
| `role`           | Role reference            | `.Values.rbac.role`      |
| `serviceAccount` | Service account reference | `.Values.serviceAccount` |

```handlebars
{{- $dependencies := dict "role" .Values.newRole "serviceAccount" .Values.newServiceAccount -}}
{{- include "hlib.deployment" (dict "context" . "dependencies" $dependencies) }}
```

#### Advanced: Template Overrides

The changes can only be added to the base template via `override` parameter, e.g.:

```handlebars
{{- include "hlib.roleBinding" (dict "context" . "override" "app.roleBinding") -}}

{{- define "app.roleBinding" -}}
metadata:
  name: {{ printf "%s-rb" (include "hlib.name" $) | trunc 63 | trimSuffix "-" }}
{{- end -}}
```
