### `hlib.serviceAccount` template

Creates Kubernetes ServiceAccount resources.

> It is considered good practice to use non-default service accounts for all applications.
> If the application does not need to communicate with the Kubernetes API,
> then from a security point of view it is better to set the `automountServiceAccountToken` parameter to `false`.

#### Basic Usage

Include this template in your chart's `templates/serviceaccount.yaml`:

```handlebars
{{- include "hlib.serviceAccount" (dict "context" .) }}
```

#### Configuration

| Parameter  | Description                                                               | Required | Default                  |
|------------|---------------------------------------------------------------------------|----------|--------------------------|
| `context`  | Root Helm context (usually `.`)                                           | Yes      | -                        |
| `values`   | Service Account configuration values (from `values.yaml`)                 | No       | `.Values.serviceAccount` |
| `override` | Name of a template that overrides the basic one configured in the library | No       | -                        |

#### Advanced: Template Overrides

The changes can only be added to the base template via `override` parameter, e.g.:

```handlebars
{{- include "hlib.serviceAccount" (dict "context" . "override" "app.sa") -}}

{{- define "app.sa" -}}
imagePullSecrets:
  - name: docker.tract-eng.com
{{- end -}}
```
