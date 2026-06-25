### `hlib.networkPolicy` template

Creates Kubernetes NetworkPolicy resources.

By default, the policy applies to the pods selected by the chart's selector labels. No `ingress`, `egress`, or `policyTypes` are configured unless explicitly provided.

#### Basic Usage

Include this template in your chart's `templates/network-policy.yaml`:

```handlebars
{{- include "hlib.networkPolicy" (dict "context" .) }}
```

#### Configuration

| Parameter  | Description                                                               | Required | Default                 |
|------------|---------------------------------------------------------------------------|----------|-------------------------|
| `context`  | Root Helm context (usually `.`)                                           | Yes      | -                       |
| `values`   | NetworkPolicy configuration values (from `values.yaml`)                   | No       | `.Values.networkPolicy` |
| `override` | Name of a template that overrides the basic one configured in the library | No       | -                       |

**Configure ingress and egress rules**

Rules are defined via `networkPolicy.ingress` and `networkPolicy.egress`, mirroring the Kubernetes API. Set `networkPolicy.policyTypes` to control which rule types apply:

```yaml
networkPolicy:
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              role: frontend
      ports:
        - protocol: TCP
          port: 80
  egress:
    - to:
        - ipBlock:
            cidr: 10.0.0.0/24
      ports:
        - protocol: TCP
          port: 5978
```

**Override the selected pods**

By default `podSelector` matches the chart's selector labels. Provide `networkPolicy.podSelector` to target a different set of pods (an empty selector via the `override` parameter selects all pods in the namespace, e.g. for a default-deny policy):

```yaml
networkPolicy:
  podSelector:
    matchLabels:
      app.kubernetes.io/component: api
```

#### Advanced: Template Overrides

The changes can only be added to the base template via `override` parameter, e.g.:

```handlebars
{{- include "hlib.networkPolicy" (dict "context" . "override" "app.networkPolicy") -}}

{{- define "app.networkPolicy" -}}
spec:
  podSelector: {}
  policyTypes:
    - Ingress
{{- end -}}
```
