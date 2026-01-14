### `hlib.cronJob` template

Creates Kubernetes CronJob manifest.
By default, only one container is deployed due to the complexity of implementing multi-containerization.

#### Basic Usage

Include this template in your chart's `templates/cron-job.yaml`:

```handlebars
{{- include "hlib.cronJob" (dict "context" . "values" .Values.cronJob) }}
```

Add the following values

```yaml
cronJob:
  schedule: "* * * * *"
  container:
    # resourceTier: "S"
    image:
      repository: busybox
```

#### Configuration

The basic template is created with some predefined non-default parameters:

```yaml
spec:
  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: Never
```

| Parameter      | Description                                                               | Required | Default           |
|----------------|---------------------------------------------------------------------------|----------|-------------------|
| `context`      | Root Helm context (usually `.`)                                           | Yes      | -                 |
| `values`       | CronJob configuration values (from `values.yaml`)                         | No       | `.Values.cronJob` |
| `envTpl`       | Name of a template that generates environment variables in YAML format    | No       | -                 |
| `dependencies` | Dictionary of dependent components (see below)                            | No       | -                 |
| `override`     | Name of a template that overrides the basic one configured in the library | No       | -                 |

**Dependency Integration**

Used to override the configuration path of resources on which it depends.

| Dependency       | Usage                                                | Default                  |
|------------------|------------------------------------------------------|--------------------------|
| `serviceAccount` | Service account name injection                       | `.Values.serviceAccount` |

```handlebars
{{- $dependencies := dict "serviceAccount" .Values.newServiceAccount -}}
{{- include "hlib.cronJob" (dict "context" . "dependencies" $dependencies) }}
```

**Environment Variables**

Provide a template name to `envTpl` that outputs environment variables in valid YAML format:

```handlebars
{{- include "hlib.cronJob" (dict "context" . "envTpl" "your.env.template") }}
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
the changes can only be added to the CronJob base template via `override` parameter as follows:

```handlebars
{{- include "hlib.cronJob" (dict "context" . "override" "app.cronJob" "envTpl" "app.cronJob.container.env") -}}

{{- define "app.cronJob" -}}
spec:
  template:
    metadata:
      annotations:
        cluster-autoscaler.kubernetes.io/safe-to-evict: "false"
{{- end -}}

{{- define "app.cronJob.container.env" -}}
TZ: {{ ((.Values.global).timezone) }}
PASSWORD:
  secretKeyRef:
    {{ include "hlib.fullname" . }}: PASSWORD
{{- end -}}
```
