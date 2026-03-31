# init

![Version: 1.29](https://img.shields.io/badge/Version-1.29-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.29](https://img.shields.io/badge/AppVersion-1.29-informational?style=flat-square)

A Helm chart for deploying Init application

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://anatolek.github.io/helm-charts | hlib | >= 0.0.0 |

## Values

### Init Application

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| init.password | string | `"SuperSecretP@ssvv0rd"` | Application password. |
| init.username | string | `"init"` | Application username. |

### Other Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| autoscaling.enabled | bool | `false` | Whether to enable Horizontal Pod Autoscaling. |
| diagnosticMode.enabled | bool | `false` | Whether to enable diagnostic mode |
| ingress.enabled | bool | `false` | Whether to enable Ingress. |
| pdb.enabled | bool | `false` | Whether to enable Pod Disruption Budget. |
| serviceAccount.enabled | bool | `false` | Whether to enable Service Account. |
