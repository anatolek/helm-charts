### `hlib.pdb` template

Creates Kubernetes PodDisruptionBudget resources.

By default, it is established that only `1` pod may be unavailable after the eviction starts.

#### Basic Usage

Include this template in your chart's `templates/pdb.yaml`:

```handlebars
{{- include "hlib.pdb" (dict "context" .) }}
```

#### Configuration

| Parameter      | Description                                                               | Required | Default       |
|----------------|---------------------------------------------------------------------------|----------|---------------|
| `context`      | Root Helm context (usually `.`)                                           | Yes      | -             |
| `values`       | Pod Disruption Budget configuration values (from `values.yaml`)           | No       | `.Values.pdb` |
| `override`     | Name of a template that overrides the basic one configured in the library | No       | -             |

#### Advanced: Template Overrides

The changes can only be added to the base template via `override` parameter, e.g.:

```handlebars
{{- include "hlib.pdb" (dict "context" . "override" "app.pdb") -}}

{{- define "app.pdb" -}}
spec:
  selector:
    matchExpressions:
      - { key: environment, operator: In, values: [dev] }
{{- end -}}
```
