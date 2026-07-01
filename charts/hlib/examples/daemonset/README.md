# daemon

![Version: 1.0](https://img.shields.io/badge/Version-1.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.0](https://img.shields.io/badge/AppVersion-1.0-informational?style=flat-square)

A Helm chart for deploying Daemon application

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://anatolek.github.io/helm-charts | hlib | >= 0.0.0 |

## Values

### Simple Application

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| daemon.postgres.host | string | `"some.postgres.test.host"` | PostgreSQL URL. |
| daemon.postgres.password | string | `"SuperSecretP@ssvv0rd"` | PostgreSQL user password. |
| daemon.postgres.username | string | `"simple"` | PostgreSQL username. |

### Other Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| diagnosticMode.enabled | bool | `false` | Whether to enable diagnostic mode |
| serviceAccount.enabled | bool | `false` | Whether to enable Service Account. |
