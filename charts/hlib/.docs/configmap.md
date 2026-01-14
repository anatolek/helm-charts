### `hlib.configMap` template

Creates Kubernetes ConfigMap resources.

It has several pre-defined annotations.

```yaml
annotations:
  "helm.sh/hook": pre-install,pre-upgrade
  "helm.sh/hook-weight": "1"
```

#### Basic Usage

Include this template in your chart's `templates/configmap.yaml`:

```handlebars
{{- include "hlib.configMap" (dict "context" .) }}
```

#### Configuration

| Parameter  | Description                                                               | Required | Default             |
|------------|---------------------------------------------------------------------------|----------|---------------------|
| `context`  | Root Helm context (usually `.`)                                           | Yes      | -                   |
| `values`   | ConfigMap configuration values (from `values.yaml`)                       | No       | `.Values.configMap` |
| `override` | Name of a template that overrides the basic one configured in the library | No       | -                   |

#### Advanced: Template Overrides

The changes can only be added to the base template via `override` parameter, e.g.:

```handlebars
{{- include "hlib.configMap" (dict "context" . "override" "app.configMap") -}}

{{- define "app.configMap" -}}
data:
  server.conf: |
    port 1194
    proto udp
    dev tun
    ca ca.crt
    cert server.crt
    key server.key
    dh dh2048.pem
    server 10.8.0.0 255.255.255.0
{{- end -}}
```
