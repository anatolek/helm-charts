# producer

![Version: 1.37](https://img.shields.io/badge/Version-1.37-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.37](https://img.shields.io/badge/AppVersion-1.37-informational?style=flat-square)

A Helm chart for deploying Producer application

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://github.com/anatolek/hlib | hlib | >= 0.0.0 |

## Values

### Producer Application

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| app.mode | string | `"producer"` | Application mode. |
| app.postgres.host | string | `""` | PostgreSQL URL. |
| app.postgres.password | string | `""` | PostgreSQL user password. |
| app.postgres.username | string | `""` | PostgreSQL user name. |

### Other Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| serviceAccount.enabled | bool | `false` | Whether to enable Service Account. |
