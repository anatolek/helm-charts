# envVars

![Version: 1.29](https://img.shields.io/badge/Version-1.29-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.29](https://img.shields.io/badge/AppVersion-1.29-informational?style=flat-square)

A Helm chart for deploying envVars application

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://github.com/anatolek/hlib | hlib | >= 0.0.0 |

## Values

### envVars Application

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| envVars.jwt.enabled | bool | `false` | Whether to validate JWT token. |
| envVars.jwt.secret | string | `"some.jwt.test.secret"` | JWT token secret. |
| envVars.kafka.producer.properties | string | `"max.poll.interval.ms=300000;session.timeout.ms=60000;heartbeat.interval.ms=3000"` | Spring Boot Kafka producer properties. |
| envVars.postgres.host | string | `"some.postgres.test.host"` | PostgreSQL URL. |
| envVars.postgres.password | string | `"SuperSecretP@ssvv0rd"` | PostgreSQL user password. |
| envVars.postgres.username | string | `"envVars"` | PostgreSQL user name. |
| envVars.pseudoBoolean | string | `"yes"` | Pseudo boolean value (string "yes", "no", "off", "on"). It must be quoted additionally in the config files. Otherwise it will be rendered as a boolean value (true/false). |
