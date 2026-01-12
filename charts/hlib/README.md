# hlib

![Version: 0.0.1](https://img.shields.io/badge/Version-0.0.1-informational?style=flat-square) ![Type: library](https://img.shields.io/badge/Type-library-informational?style=flat-square)

A reusable Helm library chart that provides common Kubernetes template primitives for building consistent, maintainable charts across GT applications.
This library eliminates code duplication by offering pre-built templates for standard Kubernetes resources including deployments, services, ingress, RBAC, jobs, and more. Charts using this library benefit from standardized configurations, predefined resource tiers, and consistent labeling patterns.

## Requirements

Kubernetes: `>=1.32.0-0`

The library may include new resource parameters that are recently added, enabled by default, and marked as at least a beta feature.
Also, some old parameters and API versions may be deprecated and their support removed from the library.

## Using the Helm library

## Values

### Container

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| CONTROLLER.container.args | tpl/list | `[]` | Arguments to the command. |
| CONTROLLER.container.command | tpl/list | `[]` | Command array to run in the container. |
| CONTROLLER.container.extraEnv | tpl/list or tpl/object | [], {} | Additional environment variables to set in the container that can be represented in a list or dictionary format. |
| CONTROLLER.container.extraPorts | tpl/list | `[]` | Additional ports for the container. |
| CONTROLLER.container.image.digest | tpl/string | `""` | Container image digest that takes precedence over the tag. |
| CONTROLLER.container.image.registry | tpl/string | `.Values.global.imageRegistry` | Container image registry. |
| CONTROLLER.container.image.repository | tpl/string | `""` | Container image repository. |
| CONTROLLER.container.image.tag | tpl/string | `""` | Container image tag. |
| CONTROLLER.container.imagePullPolicy | tpl/string | `"Always"` | Image pull policy. |
| CONTROLLER.container.lifecycle | tpl/object | `{}` | Lifecycle hooks configuration. |
| CONTROLLER.container.livenessProbe | tpl/object | `{}` | Liveness probe configuration. |
| CONTROLLER.container.name | tpl/string | `"app"` | Container name override. |
| CONTROLLER.container.port | tpl/int | `nil` | Primary container port. |
| CONTROLLER.container.readinessProbe | tpl/object | `{}` | Readiness probe configuration. |
| CONTROLLER.container.resourceTier | tpl/string | `""` | Resource tier (S, M, L, XL, 2XL, 3XL, 4XL, 5XL). it takes precedence over the "container.resources". |
| CONTROLLER.container.resources.limits.cpu | tpl/string | `""` | CPU limit for the container (optional). |
| CONTROLLER.container.resources.limits.ephemeral-storage | tpl/string | `""` | Ephemeral storage limit for the container (optional). |
| CONTROLLER.container.resources.limits.memory | tpl/string | `""` | Memory limit for the container. |
| CONTROLLER.container.resources.requests.cpu | tpl/string | `""` | Requested CPU for the container. |
| CONTROLLER.container.resources.requests.ephemeral-storage | tpl/string | `""` | Requested ephemeral storage (optional). |
| CONTROLLER.container.resources.requests.memory | tpl/string | `""` | Requested memory for the container. |
| CONTROLLER.container.securityContext | tpl/object | `{}` | Container security context. |
| CONTROLLER.container.startupProbe | tpl/object | `{}` | Startup probe configuration. |
| CONTROLLER.container.terminationMessagePath | tpl/string | `""` | Path to the termination message file. |
| CONTROLLER.container.terminationMessagePolicy | tpl/string | `""` | Policy for termination message handling. |
| CONTROLLER.container.workingDir | tpl/string | `""` | Working directory inside the container. |

### HorizontalPodAutoscaler

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| autoscaling.annotations | tpl/object | `{}` | Annotations to add to the HPA resource. |
| autoscaling.behavior | tpl/object | `{}` | HPA scaling behavior configuration. |
| autoscaling.maxReplicas | tpl/int | `nil` | Maximum number of replicas. |
| autoscaling.minReplicas | tpl/int | `nil` | Minimum number of replicas. |
| autoscaling.name | tpl/string | Release fullname | Name of the HPA resource. |
| autoscaling.targetCPUUtilizationPercentage | tpl/int | `nil` | Target average CPU utilization percentage. |
| autoscaling.targetMemoryUtilizationPercentage | tpl/int | `nil` | Target average memory utilization percentage. |

### ConfigMap

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| configMap.annotations | tpl/object | `{}` | Annotations to add to the ConfigMap metadata. |
| configMap.immutable | tpl/bool | `nil` | ensures that data stored in the ConfigMap cannot be updated. |
| configMap.name | tpl/string | Release fullname | Override the name of the ConfigMap. |

### CronJob

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| cronjob.activeDeadlineSeconds | tpl/int | `nil` | Active deadline in seconds for a job. |
| cronjob.affinity | tpl/object | `.Values.global.affinity` | Affinity settings. |
| cronjob.backoffLimit | tpl/int | `nil` | Specifies the number of retries before marking this job failed. |
| cronjob.completions | tpl/int | `nil` | Desired number of successfully finished pods the job should be run with. |
| cronjob.concurrencyPolicy | tpl/string | `""` | Specifies how to treat concurrent executions of a Job. |
| cronjob.failedJobsHistoryLimit | tpl/int | `nil` | The number of failed finished jobs to retain. |
| cronjob.imagePullSecrets | tpl/list | `.Values.global.imagePullSecrets` | Image pull secrets for pod. |
| cronjob.name | tpl/string | Release fullname | Name of the CronJob. |
| cronjob.nodeSelector | tpl/object | `.Values.global.nodeSelector` | Node selector. |
| cronjob.parallelism | tpl/int | `nil` | Maximum desired number of pods the job should run at any given time. |
| cronjob.podAnnotations | tpl/object | `{}` | Pod annotations. |
| cronjob.podDnsConfig | tpl/object | `{}` | DNS configuration for the pod. |
| cronjob.podDnsPolicy | tpl/string | `""` | DNS policy for the pod. |
| cronjob.podFailurePolicyRules | tpl/list | `[]` | Rules for handling pod failures. |
| cronjob.podHostAliases | tpl/list | `[]` | An optional list of hosts and IPs that will be injected into the pod's hosts file if specified. |
| cronjob.podHostNetwork | tpl/bool | `nil` | Enable host networking. |
| cronjob.podHostname | tpl/string | `""` | Hostname of the pod. |
| cronjob.podSecurityContext | tpl/object | `{}` | Pod security context. |
| cronjob.podShareProcessNamespace | tpl/bool | `nil` | Share process namespace between containers. |
| cronjob.priorityClassName | tpl/string | `""` | Priority class name. |
| cronjob.restartPolicy | tpl/string | `"Never"` | Restart policy for containers. |
| cronjob.runtimeClassName | tpl/string | `""` | Runtime class name. |
| cronjob.schedule | tpl/string | `nil` | CronJob schedule in Cron format (required). |
| cronjob.startingDeadlineSeconds | tpl/int | `nil` | Number of seconds after which the job is considered failed if it hasn’t started. |
| cronjob.successfulJobsHistoryLimit | tpl/int | `nil` | The number of successful finished jobs to retain. |
| cronjob.suspend | tpl/bool | `false` | Whether to suspend subsequent executions. |
| cronjob.terminationGracePeriodSeconds | tpl/integer | `nil` | Termination grace period in seconds. |
| cronjob.tolerations | tpl/list | `.Values.global.tolerations` | Tolerations. |
| cronjob.topologySpreadConstraints | tpl/list | `[]` | Topology spread constraints. |
| cronjob.ttlSecondsAfterFinished | tpl/int | `nil` | Time in seconds to retain the job after it finishes. It is recommended to use this parameter for one-time jobs instead of a Helm hook that deletes the job immediately --> "helm.sh/hook-delete-policy": hook-succeeded. |

### Deployment

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| deployment.affinity | tpl/object | `.Values.global.affinity` | Affinity rules for pod scheduling. |
| deployment.annotations | tpl/object | `{}` | Annotations to add to the deployment. |
| deployment.imagePullSecrets | tpl/list | `.Values.global.imagePullSecrets` | Image pull secrets. |
| deployment.minReadySeconds | tpl/int | `nil` | Minimum time in seconds for which a newly created pod should be ready. |
| deployment.name | tpl/string | Release fullname | Name of the deployment. |
| deployment.nodeSelector | tpl/object | `.Values.global.nodeSelector` | Node selector for pod assignment. |
| deployment.podAnnotations | tpl/object | `{}` | Annotations to add to the deployment's pod template. |
| deployment.podDnsConfig | tpl/object | `{}` | DNS configuration for pods. |
| deployment.podDnsPolicy | tpl/string | `""` | DNS policy for pods. |
| deployment.podHostAliases | tpl/list | `[]` | An optional list of hosts and IPs that will be injected into the pod's hosts file if specified. |
| deployment.podHostNetwork | tpl/bool | `nil` | Use the host's network namespace. |
| deployment.podHostname | tpl/string | `""` | Hostname of the pod. |
| deployment.podSecurityContext | tpl/object | `{}` | Security context for the pod. |
| deployment.podShareProcessNamespace | tpl/bool | `nil` | Enable process namespace sharing within pod. |
| deployment.priorityClassName | tpl/string | `""` | Priority class name for the pod. |
| deployment.replicaCount | tpl/int | `1` | Number of pod replicas (only if autoscaling is disabled). |
| deployment.revisionHistoryLimit | tpl/int | `10` | Number of old ReplicaSets to retain for rollback. |
| deployment.runtimeClassName | tpl/string | `""` | Runtime class name for the pod. |
| deployment.terminationGracePeriodSeconds | tpl/int | `nil` | Duration in seconds the pod needs to terminate gracefully. |
| deployment.tolerations | tpl/list | `.Values.global.tolerations` | Tolerations for pod scheduling. |
| deployment.topologySpreadConstraints | tpl/list | `[]` | Topology spread constraints for pods. |
| deployment.updateStrategy | tpl/object | `{}` | Strategy used to replace old Pods by new ones. |

### Diagnostic Mode

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| diagnosticMode.clusterRole.extraPermissionRules | tpl/list | `[]` | Additional RBAC cluster-wide rules required for diagnostic workloads. |
| diagnosticMode.role.extraPermissionRules | tpl/list | `[]` | Additional RBAC rules required for diagnostic workloads. |

### Global

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| global.affinity | tpl/object | `{}` | Affinity rules to apply to all pods in all workloads. |
| global.applicationPartOf | string | The name of the current chart | The name of a higher level application this one is part of. It will be added to `app.kubernetes.io/part-of`. It usually makes sense to change this for a chart with subcharts that use this Helm library. |
| global.imagePullSecrets | tpl/list | `[]` | Image pull secrets for all containers. |
| global.imageRegistry | tpl/string | `""` | Global container image registry override. |
| global.nodeSelector | tpl/object | `{}` | Node selector to apply to all pods in all workloads. |
| global.timezone | tpl/string | `""` | Default timezone to configure in containers, if applicable. |
| global.tolerations | tpl/list | `[]` | Tolerations to apply to all pods in all workloads. |

### Ingress

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| ingress.annotations | tpl/object | `{}` | Annotations to be added to the ingress. |
| ingress.className | tpl/string | `""` | Ingress class name. |
| ingress.hosts | tpl/list | `[]` | Hosts and paths rules. |
| ingress.name | tpl/string | Release fullname | Name of the ingress resource. |
| ingress.tls | tpl/list | [] | TLS configuration for ingress. |

### Job

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| job.activeDeadlineSeconds | tpl/int | `nil` | Optional duration in seconds relative to the startTime that the job may be active before being terminated. |
| job.affinity | tpl/object | `.Values.global.affinity` | Affinity rules for the Pod. |
| job.annotations | tpl/object | `{}` | Metadata annotations for the Job. |
| job.backoffLimit | tpl/int | `nil` | Number of retries before marking this job as failed. |
| job.imagePullSecrets | tpl/list | `.Values.global.imagePullSecrets` | Pull secrets for job container image. |
| job.name | tpl/string | Release fullname | Name of the Job. |
| job.nodeSelector | tpl/object | `.Values.global.nodeSelector` | Node selector for the Pod. |
| job.podAnnotations | tpl/object | `{}` | Annotations to add to the job Pod. |
| job.podDnsConfig | tpl/object | `{}` | DNS config for the Pod. |
| job.podDnsPolicy | tpl/string | `""` | DNS policy for the Pod. |
| job.podHostAliases | tpl/list | `[]` | Host aliases for the Pod. |
| job.podHostNetwork | tpl/bool | `nil` | Enable host network for the Pod. |
| job.podHostname | tpl/string | `""` | Hostname for the Pod. |
| job.podSecurityContext | tpl/object | `{}` | Security context for the Pod. |
| job.podShareProcessNamespace | tpl/bool | `nil` | Share process namespace between containers. |
| job.priorityClassName | tpl/string | `""` | Priority class name for the Pod. |
| job.restartPolicy | tpl/string | `"Never"` | Restart policy for containers in the Pod (e.g., Never or OnFailure). |
| job.runtimeClassName | tpl/string | `""` | Runtime class name for the Pod. |
| job.terminationGracePeriodSeconds | tpl/int | `nil` | Duration the Pod needs to terminate gracefully. |
| job.tolerations | tpl/list | `.Values.global.tolerations` | Tolerations for the Pod. |
| job.topologySpreadConstraints | tpl/list | `[]` | Topology spread constraints for the Pod. |
| job.ttlSecondsAfterFinished | tpl/int | `3600` | Seconds to retain the job after it finishes. |

### PodDisruptionBudget

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| pdb.annotations | tpl/object | `{}` | Annotations to add to the PodDisruptionBudget resource. |
| pdb.maxUnavailable | tpl/int | `1` | Number or percentage of pods that can be unavailable after eviction. Cannot be used with `minAvailable`. |
| pdb.minAvailable | tpl/int | `nil` | Number or percentage of pods that must be available after eviction. Cannot be used with `maxUnavailable`. |
| pdb.name | tpl/string | Release fullname | Name of the PodDisruptionBudget. |
| pdb.unhealthyPodEvictionPolicy | tpl/string | `""` | Unhealthy pod eviction policy. Valid values: "AlwaysAllow", "IfHealthyBudget". |

### ClusterRole

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| rbac.clusterRole.aggregationClusterRoleSelectors | tpl/list | `[]` | A list of label selector objects used to define aggregationRule.clusterRoleSelectors. |
| rbac.clusterRole.annotations | tpl/object | `{}` | Annotations to add to the ClusterRole metadata. |
| rbac.clusterRole.name | tpl/string | Release fullname | Override the name of the ClusterRole. |

### ClusterRoleBinding

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| rbac.clusterRoleBinding.annotations | tpl/object | `{}` | Annotations to add to the ClusterRoleBinding metadata. |
| rbac.clusterRoleBinding.name | tpl/string | Release fullname | Override the name of the ClusterRoleBinding. |

### Role

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| rbac.role.annotations | tpl/object | `{}` | Annotations to add to the Role metadata. |
| rbac.role.name | tpl/string | Release fullname | Override the name of the Role. |

### RoleBinding

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| rbac.roleBinding.annotations | tpl/object | `{}` | Annotations to add to the RoleBinding metadata. |
| rbac.roleBinding.name | tpl/string | Release fullname | Override the name of the RoleBinding. |

### Secret

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| secret.annotations | tpl/object | `{}` | Annotations to add to the Secret metadata. `{"helm.sh/hook":"pre-install,pre-upgrade","helm.sh/hook-weight":"1"}` are already added. |
| secret.immutable | tpl/bool | `nil` | ensures that data stored in the Secret cannot be updated. |
| secret.name | tpl/string | Release fullname | Override the name of the Secret. |

### Service

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| service.allocateLoadBalancerNodePorts | tpl/bool | `nil` | Allocate node ports for LoadBalancer-type Services. |
| service.annotations | tpl/object | `{}` | Annotations to add to the Service metadata. |
| service.clusterIP | tpl/string | `""` | ClusterIP address to assign to the Service. |
| service.externalIPs | tpl/list | `[]` | List of external IP addresses. |
| service.externalName | tpl/string | `""` | ExternalName for ExternalName-type Services. |
| service.externalTrafficPolicy | tpl/string | `""` | External traffic policy (e.g., Local or Cluster). |
| service.extraPorts | tpl/list | `[]` | Extra ports definitions to append to the Service. |
| service.healthCheckNodePort | tpl/int | `nil` | Health check node port. This only applies when type is set to LoadBalancer and externalTrafficPolicy is set to Local. |
| service.internalTrafficPolicy | tpl/string | `""` | Internal traffic policy. |
| service.ipFamilies | tpl/list | `[]` | IP families. Valid values are "IPv4" and "IPv6". |
| service.ipFamilyPolicy | tpl/string | `""` | IP family policy. |
| service.loadBalancerClass | tpl/string | `""` | Class of the LoadBalancer implementation. |
| service.loadBalancerIP | tpl/string | `""` | IP address for the LoadBalancer. |
| service.loadBalancerSourceRanges | tpl/list | `[]` | List of CIDR ranges allowed to access the LoadBalancer. |
| service.name | tpl/string | Release fullname | Override the name of the Service. |
| service.nodePort | tpl/int | `nil` | NodePort to expose when using NodePort/LoadBalancer. |
| service.port | tpl/int | `80` | Service port. |
| service.sessionAffinity | tpl/string | `""` | Session affinity setting. Supports "ClientIP" and "None". |
| service.sessionAffinityConfig | tpl/object | `{}` | Config for sessionAffinity. |
| service.trafficDistribution | tpl/string | `""` | Traffic distribution strategy. |
| service.type | tpl/string | `"ClusterIP"` | Service type. |

### ServiceAccount

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| serviceAccount.annotations | tpl/object | `{}` | Annotations to add to the ServiceAccount metadata. |
| serviceAccount.automountServiceAccountToken | tpl/bool | `nil` | Automatically mount the service account token. |
| serviceAccount.name | tpl/string | Release fullname | Override the name of the ServiceAccount. |

### Other Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| fullnameOverride | tpl/string | `""` | Override the chart full name. |
| nameOverride | tpl/string | `""` | Override the chart full name from `{{ Release.Name }}-{{ .Chart.Name }}` to `{{ Release.Name }}-{{ .Values.nameOverride }}`. |

