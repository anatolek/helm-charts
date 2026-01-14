### `hlib.ingress` template

Creates Kubernetes Ingress manifest.

#### Basic Usage

Include this template in your chart's `templates/ingress.yaml`:

```handlebars
{{- include "hlib.ingress" (dict "context" .) }}
```

Add the following values

```yaml
ingress:
  className: nginx-internal  # Controller-specific
  tls:
    - hosts:
        - app.example.com
      secretName: tls-cert  # Must exist in namespace
  hosts:
    - host: app.example.com
      paths:
        - path: /api
          pathType: Prefix
        - path: /static
          pathType: Exact
```

#### Configuration

| Parameter      | Description                                                               | Required | Default           |
|----------------|---------------------------------------------------------------------------|----------|-------------------|
| `context`      | Root Helm context (usually `.`)                                           | Yes      | -                 |
| `values`       | Ingress configuration values (from `values.yaml`)                         | No       | `.Values.ingress` |
| `dependencies` | Dictionary of dependent components (see below)                            | No       | -                 |
| `override`     | Name of a template that overrides the basic one configured in the library | No       | -                 |

**Dependency Integration**

Used to override the configuration path of resources on which it depends.

| Dependency | Usage                  | Default           |
|------------|------------------------|-------------------|
| `service`  | Service name injection | `.Values.service` |

```handlebars
{{- $dependencies := dict "service" .Values.newService -}}
{{- include "hlib.ingress" (dict "context" . "dependencies" $dependencies) }}
```

#### Advanced: Template Overrides

If there is no need to make any changes to the container,
the changes can only be added to the ingress base template via `override` parameter as follows:

```handlebars
{{- include "hlib.ingress" (dict "context" . "override" "app.ingress") -}}

{{- define "app.ingress" -}}
metadata:
  name: {{ printf "%s-ui" (include "hlib.fullname" $) | trunc 63 | trimSuffix "-" }}
{{- end -}}
```
