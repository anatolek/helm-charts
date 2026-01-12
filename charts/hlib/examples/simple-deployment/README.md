# simple

![Version: 1.29](https://img.shields.io/badge/Version-1.29-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.29](https://img.shields.io/badge/AppVersion-1.29-informational?style=flat-square)

A Helm chart for deploying Simple application

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://github.com/anatolek/hlib | hlib | >= 0.0.0 |

## Values

### Database Migration

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| simple.dbMigration.liquibaseAfterScripts.enabled | bool | `false` | Whether to run After Liquibase script on the PostgreSQL. |
| simple.dbMigration.liquibaseBeforeScripts.enabled | bool | `false` | Whether to run Before Liquibase script on the PostgreSQL. |

### Simple Application

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| simple.postgres.host | string | `"some.postgres.test.host"` | PostgreSQL URL. |
| simple.postgres.password | string | `"SuperSecretP@ssvv0rd"` | PostgreSQL user password. |
| simple.postgres.username | string | `"simple"` | PostgreSQL username. |

### Pre-Provision

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| simple.preProvision.enabled | bool | `false` | Whether to run pre-install PostgreSQL provision. (create a new scheme, application credentials) |
| simple.preProvision.logLevel | string | `"DEBUG"` | PostgreSQL pre-provision agent log level configuration. Valid values: FATAL, ERROR, WARN, INFO, DEBUG, TRACE. |
| simple.preProvision.postgres.password | string | `"xxxxxxx"` | PostgreSQL admin user password. |
| simple.preProvision.postgres.username | string | `"postgres"` | PostgreSQL admin username. |

### Other Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| autoscaling.enabled | bool | `false` | Whether to enable Horizontal Pod Autoscaling. |
| diagnosticMode.enabled | bool | `false` | Whether to enable diagnostic mode |
| ingress.enabled | bool | `false` | Whether to enable Ingress. |
| pdb.enabled | bool | `false` | Whether to enable Pod Disruption Budget. |
| serviceAccount.enabled | bool | `false` | Whether to enable Service Account. |
