# `hlib` Helm Library

<p align="center">
  <img width="300" src="charts/hlib/img/icon.png">
</p>

## Introduction

`hlib` is a Helm chart library.
The name comes from the Ukrainian word “хліб” (bread) — a staple on every Ukrainian table — symbolizing something essential and foundational.
Just like bread in a meal, `hlib` aims to be a reliable base ingredient for building Helm-based Kubernetes deployments.

Detailed documentation for the library is here [README.md](/charts/hlib/README.md).

It is also recommended to familiarize yourself with [BEST-PRACTICES.md](/docs/BEST-PRACTICES.md) when working with helm charts.

## Choosing a Helm Library

If you are migrating an application to Kubernetes or building a new one and don't want to spend weeks learning
Helm templating best practices, a library chart can save significant time.
Below is an honest comparison of popular options to help you decide which fits your situation.

### Feature Comparison

| Aspect                                                                                         | [hlib](https://artifacthub.io/packages/helm/hlib/hlib)                                                                                      | [Bitnami Common](https://artifacthub.io/packages/helm/bitnami/common)   | [bjw-s Common](https://github.com/bjw-s-labs/helm-charts/tree/main/charts/library/common) |
| ---------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| **Full resource templates** (Deployment, Service, Ingress, Job, CronJob, RBAC, HPA, PDB, etc.) | Yes — one-liner includes generate complete manifests                                                                                        | No — provides helper functions only; you still write manifests yourself | Yes — declarative resource definitions via values                                         |
| **Customization model**                                                                        | Template-level: consumer writes a named Go template with only the fields to change; library deep-merges it with the base (default template) | Helper functions only — consumer controls the full manifest             | Values-level only: everything configured through `values.yaml`; no template access        |
| **Can inject arbitrary / new K8s fields**                                                      | Yes — override template can add any field, even ones the library doesn't know about                                                         | Yes — consumer owns the manifest                                        | No — only fields exposed by the values schema are available                               |
| **Best-practices documentation**                                                               | Dedicated best-practices guide + examples                                                                                                   | Relies on Bitnami ecosystem docs                                        | Getting-started guide                                                                     |
| **Consumer writes**                                                                            | Minimal override templates (Go/YAML) + `values.yaml`                                                                                        | Full Kubernetes manifests + helper includes                             | `values.yaml` only — no templates needed                                                  |
| **Flexibility ceiling**                                                                        | Very high — anything expressible in Kubernetes YAML                                                                                         | Unlimited — full control of manifests                                   | Bounded by what the values schema exposes                                                 |
| **Community size**                                                                             | Small (new project)                                                                                                                         | Very large (part of Bitnami ecosystem)                                  | Medium (popular in homelab/self-hosting)                                                  |

### When to Choose Each

| Choose             | When                                                                                                                                                                                                                                                                                                                                                            |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **hlib**           | You want to deploy standard workloads (Deployments, Jobs, CronJobs) to a cluster with minimal boilerplate. You prefer one-liner template includes that produce complete manifests with sensible defaults, unified resource tiers, and don't want to write Kubernetes YAML from scratch. Good fit for teams standardizing Helm charts across multiple services. |
| **Bitnami Common** | You are already using **Bitnami application charts** (PostgreSQL, Redis, Kafka, etc.) and want consistency with their ecosystem. Note: Bitnami Common provides helper functions (labels, names, images, capabilities), not full resource templates — you still write your own manifests.                                                                        |
| **bjw-s Common**   | You need a highly **flexible, values-driven** approach where almost everything is configured through `values.yaml` rather than template files. Popular choice for **homelab and self-hosting** setups where a single chart definition deploys many different applications.                                                                                      |

## Getting started

### Git Configuration:

- `Chart.yaml`: To include `hlib` dependency, it is needed to add a dependencies section to this file, for example

    ```yaml
    apiVersion: v2
    description: Helm chart to deploy special service
    name: special
    version: 1.0.0
    dependencies:
      - name: hlib
        version: <desired_library_version>
        repository: https://anatolek.github.io/helm-charts
    ```

- `.helmignore`: It is important to exclude unnecessary files and directories from your chart package.
  Add it to the helm chart folder.
    <details>
    <summary>.helmignore</summary>

    ```text
    # Patterns to ignore when building packages.
    # This supports shell glob matching, relative path matching, and
    # negation (prefixed with !). Only one pattern per line.
    .DS_Store
    # Common VCS dirs
    .git/
    .gitignore
    .bzr/
    .bzrignore
    .hg/
    .hgignore
    .svn/
    # Common backup files
    *.swp
    *.bak
    *.tmp
    *.orig
    *~
    # Helm
    .helmignore
    # Various IDEs
    .project
    .idea/
    *.tmproj
    .vscode/
    ```
    </details>

- `.gitignore`: It is important to include the `charts/` in this file to avoid including unnecessary duplicate content in a git repository.
  This file is usually located in the root of the repository.

    <details>
    <summary>.gitignore</summary>

    ```text
    # .gitignore
    charts/
    ```
    </details>

- `Chart.lock`: It locks the versions of the dependencies.
  Run `helm dependency update` to generate this file. This is good practice to commit it to your Git repository.

### Template Usage

After all the settings are done, you can start using the basic templates, variables, and helper functions defined in the library.
Some examples are described in the `charts/hlib/examples/` folder.
You can take them as a basis.

### Create Helm Package

Issue the following commands to add the Helm repository that contains the library chart, and update dependencies:

```shell
helm repo add hlib https://anatolek.github.io/helm-charts
helm dependency build <helm_chart_location>
helm package <helm_chart_location>
```

#### `helm dependency` commands explanation

| Command                  | Purpose                                               | Modifies `Chart.lock`? | When to Use                                                                                                                   |
|--------------------------|-------------------------------------------------------|------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| `helm dependency update` | Resolves and downloads dependencies from `Chart.yaml` | ✅ Yes                 | When you want to add or change dependencies in `Chart.yaml`                                                                   |
| `helm dependency build`  | Builds the `charts/` directory from `Chart.lock`      | ❌ No                  | When you already have a `Chart.lock` and want to rebuild dependencies exactly. This usually happens through a build pipeline. |

## Contributions

### Versioning rules

Chart version contains three positions `X.Y.Z`

`Z` - should be increased **for fixes**

`Y` - should be increased for code update with **adding new features**

`X` - should be increased if update **breaks backward compatibility**

### Documentation-related changes

*README.md* chart files are generated using a [helm-docs](https://github.com/norwoodj/helm-docs/tree/master) utility.
Don't try to add changes directly.

```shell
helm-docs --skip-version-footer
```

- Add documentation on variables to `values.yaml`
- A new section should be added as a separate file to `.docs/`, and then linked into the template `README.md.gotmpl`.

### Values schema changes

The `values.schema.json` file is automatically generated by the [Helm schema plugin](https://github.com/losisin/helm-values-schema-json)

```shell
cd charts/hlib/
helm schema --use-helm-docs
```
