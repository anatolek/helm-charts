### `hlib.secret` template

Creates Kubernetes Secret resources.

It has several pre-defined annotations.

```yaml
annotations:
  "helm.sh/hook": pre-install,pre-upgrade
  "helm.sh/hook-weight": "1"
```

#### Basic Usage

Include this template in your chart's `templates/secret.yaml`:

```handlebars
{{- include "hlib.secret" (dict "context" .) }}
```

#### Configuration

| Parameter  | Description                                                               | Required | Default          |
|------------|---------------------------------------------------------------------------|----------|------------------|
| `context`  | Root Helm context (usually `.`)                                           | Yes      | -                |
| `values`   | Secret configuration values (from `values.yaml`)                          | No       | `.Values.secret` |
| `override` | Name of a template that overrides the basic one configured in the library | No       | -                |

#### Advanced: Template Overrides

The changes can only be added to the base template via `override` parameter, e.g.:

```handlebars
{{- include "hlib.secret" (dict "context" . "override" "app.secret") -}}

{{- define "app.secret" -}}
data:
  PASSWORD: "P@ssvv0rd"
{{- end -}}
```
