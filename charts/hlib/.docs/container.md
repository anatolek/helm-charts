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
  # resourceTier: "m250Mi-c250m"
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

**Resource tier (`resourceTier`)**

You can set a compact string instead of spelling out `container.resources`. The format is **`m<memory>-c<cpu>`** where the leading `m` / `c` letters are case-insensitive. Memory and CPU values must be valid Kubernetes quantities (same as in a Pod spec).

Examples:

```yaml
container:
  resourceTier: "m100Mi-c100m"
```

```yaml
container:
  resourceTier: "M100M-C1"
```

Rendered resources:

- **Requests:** `memory` and `cpu` from the string; `ephemeral-storage` is always `50Mi`.
- **Limits:** `memory` matches requests; `ephemeral-storage` is always `2Gi`. **CPU is not set in limits** (only in requests).

**Precedence:** If `container.resources.requests` is set, that block is used and `resourceTier` is ignored. Use either full `container.resources` or `resourceTier`, or set resources when you need to override a tier for a specific environment.

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
