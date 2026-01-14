## Predefined functions

These are auxiliary functions defined in the library that can be reused wherever needed.

### `hlib.util.merge`

Merges two YAML snippets rendered by template includes.

**Inputs:**

- `.override` (string): Name of the override template to include and parse.
- `.base` (string): Name of the base template to include and parse.
- `.context` (dict): Values context for evaluating `.override`.

The override values take precedence over the base.

**Usage:**

```handlebars
{{ include "hlib.util.merge" (dict "override" "template.override" "base" "template.base" "context" .) }}
```

### `hlib.util.renderEnvVars`

Renders Kubernetes environment variable definitions based on input dictionary.

**Inputs:**

- `.env` (dict): A map of environment variable keys to values or configuration objects.
- `.context` (dict): Optional context for evaluating values.

**Supported value formats:**

- List-formatted values ("slice") rendered as is.

  Example:

  ```yaml
  - name: FOO
    value: bar
  ```

  Result:

  ```yaml
  - name: "FOO"
    value: "bar"
  ```

- Simple values (string, int, float, bool) rendered as plain `value: "<string>"`.

  Example:
  ```yaml
  FOO: "bar"
  ```

  Result:

  ```yaml
  - name: "FOO"
    value: "bar"
  ```

- Secret references.

  Example:

  ```yaml
  SECRET:
    secretKeyRef:
      my-secret: password
  ```

  Result:

  ```yaml
  - name: "SECRET"
    valueFrom:
      secretKeyRef:
        name: "my-secret"
        key: "password"
  ```

- ConfigMap references.

  Example:

  ```yaml
  CONFIG:
    configMapKeyRef:
      my-config: setting
  ```

  Result:

  ```yaml
  - name: "CONFIG"
    valueFrom:
      configMapKeyRef:
        name: "my-config"
        key: "setting"
  ```

**Ignored:**

Entries with a value of `<no value>` or empty/null will be skipped.

**Usage:**

```handlebars
{{ include "hlib.util.renderEnvVars" (dict "env" .Values.env "context" .) }}
```

### `hlib.util.image`

Renders a full image reference string using registry, repository, tag, or digest.

**Inputs:**

- `.imageRoot.repository` (string): Required. The image repository (e.g., "nginx").
- `.imageRoot.tag` (string, optional): The image tag (e.g., "1.23.4"). Falls back to .chart.AppVersion if not set.
- `.imageRoot.digest` (string, optional): The image digest (e.g., "sha256:abcd1234"). Overrides tag if set.
- `.imageRoot.registry` (string, optional): The image registry (e.g., "ghcr.io/myorg"). Can also be overridden globally.
- `.global.imageRegistry` (string, optional): Fallback registry if .imageRoot.registry is not set.
- `.chart.AppVersion` (string, optional): Used as fallback tag if neither tag nor digest is provided.

**Behavior:**

- Uses "@" as separator if digest is provided, ":" otherwise.
- If both registry and digest are set, e.g. `ghcr.io/myorg/app@sha256:abc123`
- If tag is missing and digest is not provided, defaults to chart AppVersion.

**Examples:**

Input:

```yaml
imageRoot:
  repository: myapp
  tag: "2.0.1"
  registry: "ghcr.io/myteam"
```

Output:

```yaml
ghcr.io/myteam/myapp:2.0.1
```

**Usage:**

```handlebars
{{ include "hlib.util.image" (dict "imageRoot" .Values.path.to.the.image "global" .Values.global "chart" .Chart) }}
```

### `hlib.util.tplrender`

This template evaluates a string or YAML object as a Helm template using the `tpl` function.

**Inputs:**

- `value` (any): The string or YAML structure to render (can contain Go template expressions like {{ .Values.* }}).
- `context` (dict): The context (usually `.`) for evaluating templates.
- `scope` (dict): (optional) The scope to be used inside a `with` block for relative references.

**Behavior:**

- If `value` is a plain string, it is evaluated with `tpl`.
- If `value` is a YAML map or list, it is first serialized to YAML or JSON.
- If the rendered content contains `{{`, `tpl` is used to evaluate it.
- If `scope` is provided, it wraps the template in a `with` block using `RelativeScope`.
  Use `scope` when the template string references variables not in the top-level context (like .Values.*),
  but in local or nested structures like iteration items, condition blocks, or subfields.

**Examples:**

1. Rendering a plain templated string:
   ```handlebars
   {{ include "hlib.util.tplrender" (dict "value" .Values.path.to.the.Value "context" $) }}
   ```

2. Rendering YAML with templates and a scope:
   ```handlebars
   {{ include "hlib.util.tplrender" (dict "value" .Values.path.to.the.Value "context" $ "scope" $app) }}
   ```

**Output:**

The resulting evaluated template as a string or YAML structure.

### `hlib.util.springKafkaValues`

This template renders prefixed Kafka values either as:

1. A single value.

   Example:

   ```handlebars
   {{ include "hlib.util.springKafkaValues" (dict "prefix" "spring.kafka.listener.concurrency" "values" "3") }}
   ```

   Output:

   ```yaml
   "spring.kafka.listener.concurrency": "3"
   ```

2. A list of key=value pairs separated by semicolons.

   Example:

   ```handlebars
   {{ include "hlib.util.springKafkaValues" (dict "prefix" "spring.kafka" "values" "listener.concurrency=3;consumer.auto-offset-reset=earliest") }}
   ```

   Output:

   ```yaml
   "spring.kafka.listener.concurrency": "3"
   "spring.kafka.consumer.auto-offset-reset": "earliest"
   ```

### `hlib.default-check-required-msg`

A default message string to be used when checking for a required value

**Usage:**

```handlebars
{{- $requiredMsg := include "hlib.default-check-required-msg" $ -}}
{{ required (printf $requiredMsg "SOME.VALUE.NAME") .Values.some.value }}
```
