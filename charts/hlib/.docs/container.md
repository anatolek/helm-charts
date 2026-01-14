### `hlib.container` template

Creates Kubernetes container specification.

#### Basic Usage

Include within your controller template (e.g., deployment):

```handlebars
containers:
- {{- include "hlib.container" (dict "context" . "values" .Values.container) }}
```

Add the following values

```yaml
container:
  # resourceTier: "M"
  image:
    repository: busybox
```

#### Configuration

| Parameter      | Description                                                               | Required | Default                        |
|----------------|---------------------------------------------------------------------------|----------|--------------------------------|
| `context`      | Root Helm context (usually `.`)                                           | Yes      | -                              |
| `values`       | Container configuration values (from `values.yaml`)                       | No       | `.Values.deployment.container` |
| `env`          | Environment variables to add to the container                             | No       | -                              |
| `override`     | Name of a template that overrides the basic one configured in the library | No       | -                              |

**Environment Variables**

You can assign container values to `env` parameter directly, e.g.

```handlebars
containers:
- {{- include "hlib.container" (dict "context" . "env" (fromYaml "FOO: true\nBAR: OK")) }}
```

or as a rendered template

```handlebars
...
{{- $env := fromYaml (include "container.env" .) -}}
spec:
  template:
    spec:
      containers:
      - {{- include "hlib.container" (dict "context" . "env" $env) }}
...

{{- define "container.env" -}}
FOO: true
BAR: OK
{{- end -}}
```

Combine pre-rendered `env` + `extraEnv`:

```yaml
container:
  extraEnv:
    LOG_LEVEL: debug
```

**Resource Tiers**

Predefined resource profiles (avoids manual CPU/memory config):

```yaml
container:
  resourceTier: "L"  # Options: S, M, L, XL, 2XL-5XL
```

| Resource Tier | Request Memory | Request CPU | Request Ephemeral Storage | Limit Memory | Limit CPU | Limit Ephemeral Storage |
|---------------|----------------|-------------|---------------------------|--------------|-----------|-------------------------|
| S             | 100Mi          | 100m        | 50Mi                      | 100Mi        | -         | 2Gi                     |
| M             | 250Mi          | 250m        | 50Mi                      | 250Mi        | -         | 2Gi                     |
| L             | 500Mi          | 500m        | 50Mi                      | 500Mi        | -         | 2Gi                     |
| XL            | 1Gi            | 1           | 50Mi                      | 1Gi          | -         | 2Gi                     |
| 2XL           | 2Gi            | 1           | 50Mi                      | 2Gi          | -         | 2Gi                     |
| 3XL           | 4Gi            | 2           | 50Mi                      | 4Gi          | -         | 2Gi                     |
| 4XL           | 8Gi            | 2           | 50Mi                      | 8Gi          | -         | 2Gi                     |
| 5XL           | 16Gi           | 4           | 50Mi                      | 16Gi         | -         | 2Gi                     |

Custom resources can be configured when `container.resourceTier` is not used.

#### Advanced: Template Overrides

The changes can only be added to the container base template via `override` parameter.
For example, if it is needed to add a secret resource as input variables for container:

```handlebars
{{- include "hlib.deployment" (dict "context" . "override" "app.deployment") -}}

{{- define "app.deployment" -}}
spec:
  template:
    metadata:
      annotations:
        checksum/secret: {{ include (print .Template.BasePath "/secret.yaml") $ | sha256sum }}
    spec:
      containers:
      - {{- include "hlib.container" (dict "context" . "override" "app.deployment.container") }}
{{- end -}}

{{- define "app.deployment.container" -}}
envFrom:
  - secretRef:
      name: {{ include "hlib.fullname" . }}
{{- end -}}
```
