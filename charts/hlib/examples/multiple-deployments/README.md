# multi-depl

![Version: 1.29](https://img.shields.io/badge/Version-1.29-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.29](https://img.shields.io/badge/AppVersion-1.29-informational?style=flat-square)

A Helm chart for deploying multi-depl application

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://github.com/anatolek/hlib | hlib | >= 0.0.0 |

## Values

### multiDepl Application

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| multiDepl.postgres.host | string | `"some.postgres.test.host"` | PostgreSQL URL. |
| multiDepl.postgres.password | string | `"SuperSecretP@ssvv0rd"` | PostgreSQL user password. |
| multiDepl.postgres.username | string | `"simple"` | PostgreSQL user name. |
