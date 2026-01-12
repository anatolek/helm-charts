# subchart

![Version: 1](https://img.shields.io/badge/Version-1-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1](https://img.shields.io/badge/AppVersion-1-informational?style=flat-square)

A Helm chart for deploying SubChart application

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://github.com/anatolek/hlib | consumer | >= 0.0.0 |
| https://github.com/anatolek/hlib | producer | >= 0.0.0 |

## Values

### SubChart Application

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| global.postgres.host | string | `"some.postgres.test.host"` | PostgreSQL URL. |
| global.postgres.password | string | `"SuperSecretP@ssvv0rd"` | PostgreSQL user password. |
| global.postgres.username | string | `"subchart-dbuser"` | PostgreSQL user name. |

### Other Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| diagnosticMode.enabled | bool | `false` | Whether to enable diagnostic mode |
