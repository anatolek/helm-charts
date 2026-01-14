# Best practices for configuring helm charts

These are just recommendations.
It is always your choice whether to use them or not.

## Service Account

From a security point of view, it is recommended not to use the default account, but to use a specific one.
If access to the Kubernetes API is not needed, then for additional security hardening it is recommended to specify `automountServiceAccountToken: false`.

## Do not specify resources in the chart

Do not specify resources in the default chart `values.yaml` to avoid difficulties when it is needed to redefine them.

 :x:

```yaml
deployment:
  container:
    resourceTier: "S"
```

 :x:

```yaml
deployment:
  container:
    resources:
      requests:
        memory: "1Gi"
        cpu: "1"
        ephemeral-storage: "50Mi"
      limits:
        memory: "1"
        ephemeral-storage: "2Gi"
```

## Add the ability to configure the system timezone

This should be done by adding the special [TZ](https://www.gnu.org/software/libc/manual/html_node/TZ-Variable.html) environment variable to all containers.

```yaml
env:
  - name: TZ
    value: {{ ((.Values.gloal).timezone) }}
```

## Describe new variables

Describe new application-specific variables in the [helm-docs](https://github.com/norwoodj/helm-docs) format within `values.yaml`, e.g.

```yaml
app:
  postgres:
    # -- PostgreSQL URL.
    # @section -- New Application
    host: some.postgres.test.host
```

Ignore library-specific variables since they are already described in the library documentation

```yaml
# @ignored
deployment:
  container:
    port: 8080
      image:
        repository: nginx
```

Don't add the library variables to your chart `values.yaml` unless you plan to override them.

## Specify all probes for the application

- **Startup Probe**: required if the application takes longer than ~10 seconds to launch.
- **Liveness Probe**: check if the application is live itself.
- **Readiness Probe**: сheck if the application can send traffic, for example, by checking the connection to the database
from the application.

## Set correct variable names

Follow these [Naming Conventions](https://helm.sh/docs/chart_best_practices/values/#naming-conventions)

## Use appropriate image configuration

The container template already uses a common approach for image configuration provided by [Bitnami](https://bitnami.com/).
Therefore, recommended to use it. Read more about `hlib.util.image` in the [documentation](../charts/hlib/README.md) if you need more info.

 :x:

```yaml
deployment:
  container:
    image:
      repository: quay.io/nginx/nginx-ingress
```

 :white_check_mark:

 ```yaml
deployment:
  container:
    image:
      registry: quay.io/nginx
      repository: nginx-ingress
```

## Avoid renaming resources

By default, all resources are named {{ include "hlib.fullname" . }} in the library.
If possible, try not to rename them, but if you have to rename them (for example, if you need 2 resources of the same type), do it correctly,e.g.

```handlebars
{{ printf "%s-some-suffix" (include "hlib.name" .) | trunc 63 | trimSuffix "-" }}
```

## Adding a `quote` and an `if-end` block are unnecessary when specifying variables

 :warning:

```yaml
{{- define "app.deployment.container.env" -}}
POSTGRES_HOST: {{ .Values.db.postgres.host }} # ok
POSTGRES_PORT: {{ .Values.db.postgres.port | quote }} # quoting isn't necessary
{{- end -}}
```

`hlib.util.renderEnvVars` does all the magic for you

 :warning:

```yaml
{{- define "app.deployment.container.env" -}}
{{- if .Values.app.encryptKey }}
ENCRYPT_KEY: {{ .Values.app.encryptKey }}
{{- end }}
{{- end -}}
```

`hlib.util.renderEnvVars` does not include the variable to the container env configuration if the value is empty

## Container and service ports are different

 :x:

```yaml
{{- define "app.deployment.container" -}}
ports:
  - containerPort: {{ .Values.service.port }}
{{- end -}}
```

 :white_check_mark: 1st option

```yaml
# template/deployment.yaml
{{- define "app.deployment.container" -}}
ports:
  - containerPort: 8080
{{- end -}}
```

 :white_check_mark: 2nd option

```yaml
# values.yaml
deployment:
  container:
    port: 8080
```

The container port is specified by the application developer and cannot potentially be changed.
The service port, on the other hand, may change depending on the network configuration in the cluster.

It is recommended to specify:

- Container port > `1024`
- Service port = `80` (default)

## Use default port names if possible

Default port names:

- Container port name: `cont-tcp-port`
- Service port name: `svc-tcp-port`

 :x:

```yaml
{{- define "app.deployment.container" -}}
ports:
  - name: {{ .Chart.Name }}-api
{{- end -}}
```

The container and service name are limited to **15 (!)** characters.
Therefore, it is necessary to consider names that may change;
they should be replaced with static names or used `trim` function.

## Removing helper job containers after completion

It is recommended to add `spec.ttlSecondsAfterFinished` > `0` parameter for one-time helper jobs (e.g. database migration)
instead of a Helm hook that deletes the job immediately (`"helm.sh/hook-delete-policy": hook-succeeded`).
This will help to check the job logs in case of a malfunction.

 :white_check_mark:

```yaml
# values.yaml
job:
  ttlSecondsAfterFinished: 7200 # default library value is 3600
```

## Update controller configuration when ConfigMaps or Secrets change

 :white_check_mark:

```yaml
# values.yaml
{{- define "app.deployment" -}}
spec:
  template:
    metadata:
      annotations:
        checksum/secret: {{ include (print .Template.BasePath "/secret.yaml") $ | sha256sum }}
        checksum/config: {{ include (print .Template.BasePath "/configmap.yaml") $ | sha256sum }}
{{- end -}}
```

## Remove duplicate configuration

There is no point in overriding the same code that already exists in the library.

 :x:

```yaml
{{- define "app.secret" -}}
metadata:
  annotations:
    "helm.sh/hook": pre-install,pre-upgrade
    "helm.sh/hook-weight": "1"
stringData:
  JWT_SECRET: {{ .Values.jwt.secret }}
{{- end -}}
```

 :white_check_mark:

```yaml
{{- define "app.secret" -}}
stringData:
  JWT_SECRET: {{ .Values.jwt.secret }}
{{- end -}}
```
