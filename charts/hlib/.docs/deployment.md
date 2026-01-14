### `hlib.deployment` template

Creates Kubernetes Deployment manifest.
By default, only one container is deployed due to the complexity of implementing multi-containerization.

#### Basic Usage

Include this template in your chart's `templates/deployment.yaml`:

```handlebars
{{- include "hlib.deployment" (dict "context" . "values" .Values.deployment) }}
```

Add the following values

```yaml
deployment:
  container:
    # resourceTier: "S"
    port: 8080
    image:
      repository: nginx
```

#### Configuration

| Parameter      | Description                                                               | Required | Default              |
|----------------|---------------------------------------------------------------------------|----------|----------------------|
| `context`      | Root Helm context (usually `.`)                                           | Yes      | -                    |
| `values`       | Deployment configuration values (from `values.yaml`)                      | No       | `.Values.deployment` |
| `envTpl`       | Name of a template that generates environment variables in YAML format    | No       | -                    |
| `dependencies` | Dictionary of dependent components (see below)                            | No       | -                    |
| `override`     | Name of a template that overrides the basic one configured in the library | No       | -                    |

**Dependency Integration**

Used to override the configuration path of resources on which it depends.

| Dependency       | Usage                                                | Default                  |
|------------------|------------------------------------------------------|--------------------------|
| `autoscaling`    | Auto-scaling status (disables replicas when enabled) | `.Values.autoscaling`    |
| `serviceAccount` | Service account name injection                       | `.Values.serviceAccount` |

```handlebars
{{- $dependencies := dict "autoscaling" .Values.newAutoscaling "serviceAccount" .Values.newServiceAccount -}}
{{- include "hlib.deployment" (dict "context" . "dependencies" $dependencies) }}
```

**Environment Variables**

Provide a template name to `envTpl` that outputs environment variables in valid YAML format:

```handlebars
{{- include "hlib.deployment" (dict "context" . "envTpl" "your.env.template") }}
```

Example template (`templates/_env.yaml`):
```handlebars
{{- define "your.env.template" -}}
- name: ENV_VAR
  value: "production"
{{- end -}}

# or use a simplified approach

{{- define "your.env.template" -}}
ENV_VAR: "production"
{{- end -}}
```

#### Advanced: Template Overrides

If there is no need to make any changes to the container,
the changes can only be added to the deployment base template via `override` parameter as follows:

```handlebars
{{- include "hlib.deployment" (dict "context" . "override" "app.deployment" "envTpl" "app.deployment.container.env") -}}

{{- define "app.deployment" -}}
spec:
  template:
    metadata:
      annotations:
        checksum/secret: {{ include (print .Template.BasePath "/secret.yaml") $ | sha256sum }}
{{- end -}}

{{- define "app.deployment.container.env" -}}
TZ: {{ ((.Values.global).timezone) }}
PASSWORD:
  secretKeyRef:
    {{ include "hlib.fullname" . }}: PASSWORD
{{- end -}}
```
