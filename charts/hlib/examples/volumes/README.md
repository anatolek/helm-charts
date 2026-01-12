# vol

![Version: 1.29](https://img.shields.io/badge/Version-1.29-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.29](https://img.shields.io/badge/AppVersion-1.29-informational?style=flat-square)

A Helm chart for deploying Vol application

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://github.com/anatolek/hlib | hlib | >= 0.0.0 |

## Values

### Vol Application

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| vol.encryptionKey | string | `"3xKCUpEFOfqDPoJcN22yaUYhUSs7S7SWQyzL74ZMGRo="` | Encription key. It must be 32 bytes long. |
