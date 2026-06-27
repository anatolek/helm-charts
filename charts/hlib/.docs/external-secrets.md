### `hlib.externalSecrets` template

Creates [External Secrets Operator](https://external-secrets.io/) `ExternalSecret` resources (`external-secrets.io/v1`).

#### Basic Usage

Include this template in your chart's `templates/external-secret.yaml`:

```handlebars
{{- include "hlib.externalSecrets" (dict "context" .) }}
```

The spec is rendered from the `externalSecrets` values, e.g.:

```yaml
externalSecrets:
  refreshInterval: "1h"
  secretStoreRef:
    name: aws-secrets-manager
    kind: ClusterSecretStore
  target:
    creationPolicy: Owner
  data:
    - secretKey: token
      remoteRef:
        key: prod/app
        property: token
```

#### Configuration

| Parameter  | Description                                                               | Required | Default                  |
|------------|---------------------------------------------------------------------------|----------|--------------------------|
| `context`  | Root Helm context (usually `.`)                                           | Yes      | -                        |
| `values`   | ExternalSecret configuration values (from `values.yaml`)                  | No       | `.Values.externalSecrets`|
| `override` | Name of a template that overrides the basic one configured in the library | No       | -                        |

#### Advanced: Template Overrides

The changes can only be added to the base template via `override` parameter, e.g.:

```handlebars
{{- include "hlib.externalSecrets" (dict "context" . "override" "app.externalSecrets") -}}

{{- define "app.externalSecrets" -}}
spec:
  dataFrom:
    - extract:
        key: prod/app
{{- end -}}
```
