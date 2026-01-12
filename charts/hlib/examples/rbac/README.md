# rbac

![Version: 0.0.1](https://img.shields.io/badge/Version-0.0.1-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 0.0.1](https://img.shields.io/badge/AppVersion-0.0.1-informational?style=flat-square)

A Helm chart for deploying RBAC application

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://github.com/anatolek/hlib | hlib | >= 0.0.0 |

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| rbac.enabled | bool | `true` | Whether to enable Role-based access control. |
| serviceAccount.enabled | bool | `true` | Whether to enable Service Account. |
