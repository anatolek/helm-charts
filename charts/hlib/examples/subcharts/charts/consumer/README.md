# consumer

![Version: 1.29](https://img.shields.io/badge/Version-1.29-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.29](https://img.shields.io/badge/AppVersion-1.29-informational?style=flat-square)

A Helm chart for deploying Consumer application

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://github.com/anatolek/hlib | hlib | >= 0.0.0 |

## Values

### Consumer Application

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| app.mode | string | `"consumer"` | Application mode. |
| app.postgres.host | string | `""` | PostgreSQL URL. |
| app.postgres.password | string | `""` | PostgreSQL user password. |
| app.postgres.username | string | `""` | PostgreSQL user name. |

### Other Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| autoscaling.enabled | bool | `false` | Whether to enable Horizontal Pod Autoscaling. |
| pdb.enabled | bool | `false` | Whether to enable Pod Disruption Budget. |
| serviceAccount.enabled | bool | `false` | Whether to enable Service Account. |
