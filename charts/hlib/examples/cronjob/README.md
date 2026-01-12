# cronj

![Version: 1.37](https://img.shields.io/badge/Version-1.37-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.37](https://img.shields.io/badge/AppVersion-1.37-informational?style=flat-square)

A Helm chart for deploying CronJ application

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://github.com/anatolek/hlib | hlib | >= 0.0.0 |

## Values

### Simple Application

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| cronj.postgres.host | string | `"some.postgres.test.host"` | PostgreSQL URL. |
| cronj.postgres.password | string | `"SuperSecretP@ssvv0rd"` | PostgreSQL user password. |
| cronj.postgres.username | string | `"simple"` | PostgreSQL username. |

### Other Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| diagnosticMode.enabled | bool | `false` | Whether to enable diagnostic mode |
| serviceAccount.enabled | bool | `false` | Whether to enable Service Account. |
