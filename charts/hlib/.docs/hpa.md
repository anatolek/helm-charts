### `hlib.hpa` template

Creates Kubernetes HorizontalPodAutoscaler resources.

#### Basic Usage

Include this template in your chart's `templates/hpa.yaml`:

```handlebars
{{- include "hlib.hpa" (dict "context" .) }}
```

#### Configuration

| Parameter      | Description                                                               | Required | Default               |
|----------------|---------------------------------------------------------------------------|----------|-----------------------|
| `context`      | Root Helm context (usually `.`)                                           | Yes      | -                     |
| `values`       | Autoscaling configuration values (from `values.yaml`)                     | No       | `.Values.autoscaling` |
| `dependencies` | Dictionary of dependent components (see below)                            | No       | -                     |
| `override`     | Name of a template that overrides the basic one configured in the library | No       | -                     |

**Dependency Integration**

Used to override the configuration path of resources on which it depends.

| Dependency   | Usage                | Default              |
|--------------|----------------------|----------------------|
| `deployment` | Deploymnet reference | `.Values.deployment` |

```handlebars
{{- $dependencies := dict "deployment" .Values.newDeployment -}}
{{- include "hlib.hpa" (dict "context" . "dependencies" $dependencies) }}
```

#### Advanced: Template Overrides

The changes can only be added to the base template via `override` parameter, e.g.:

```handlebars
{{- include "hlib.hpa" (dict "context" . "override" "app.hpa") -}}

{{- define "app.hpa" -}}
apiVersion: autoscaling/v1
{{- end -}}
```
