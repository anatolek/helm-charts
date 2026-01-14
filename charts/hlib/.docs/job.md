### `hlib.job` template

Creates Kubernetes Job manifest.
By default, only one container is deployed due to the complexity of implementing multi-containerization.

#### Basic Usage

Include this template in your chart's `templates/job.yaml`:

```handlebars
{{- include "hlib.job" (dict "context" . "values" .Values.job) }}
```

Add the following values

```yaml
job:
  container:
    # resourceTier: "S"
    port: 8080
    image:
      repository: nginx
```

#### Configuration

The basic template is created with some predefined non-default parameters:

```yaml
spec:
  # It is recommended to use this parameter for one-time jobs instead of a Helm hook
  # that deletes the job immediately --> "helm.sh/hook-delete-policy": hook-succeeded
  ttlSecondsAfterFinished: 3600  # Completed job will be deleted after 1 hour.
  template:
    spec:
      restartPolicy: Never
```

| Parameter      | Description                                                               | Required | Default       |
|----------------|---------------------------------------------------------------------------|----------|---------------|
| `context`      | Root Helm context (usually `.`)                                           | Yes      | -             |
| `values`       | Job configuration values (from `values.yaml`)                             | No       | `.Values.job` |
| `envTpl`       | Name of a template that generates environment variables in YAML format    | No       | -             |
| `dependencies` | Dictionary of dependent components (see below)                            | No       | -             |
| `override`     | Name of a template that overrides the basic one configured in the library | No       | -             |

**Dependency Integration**

Used to override the configuration path of resources on which it depends.

| Dependency       | Usage                                                | Default                  |
|------------------|------------------------------------------------------|--------------------------|
| `serviceAccount` | Service account name injection                       | `.Values.serviceAccount` |

```handlebars
{{- $dependencies := dict "serviceAccount" .Values.newServiceAccount -}}
{{- include "hlib.job" (dict "context" . "dependencies" $dependencies) }}
```

**Environment Variables**

Provide a template name to `envTpl` that outputs environment variables in valid YAML format:

```handlebars
{{- include "hlib.job" (dict "context" . "envTpl" "your.env.template") }}
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
the changes can only be added to the job base template via `override` parameter as follows:

```handlebars
{{- include "hlib.job" (dict "context" . "override" "app.job" "envTpl" "app.job.container.env") -}}

{{- define "app.job" -}}
metadata:
  name: {{ printf "%s-pre-deploy" (include "hlib.fullname" $) | trunc 63 | trimSuffix "-" }}
{{- end -}}

{{- define "app.job.container.env" -}}
TZ: {{ ((.Values.global).timezone) }}
{{- end -}}
```
