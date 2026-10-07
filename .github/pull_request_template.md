## Summary

<!-- Explain the problem, what changed, and any user-facing or compatibility impact. -->

## Related issue

<!-- Use "Closes #<number>" when this PR fully resolves an issue. -->

## Validation

<!-- List commands and results. Explain checks that are not applicable or were not run. -->

## Checklist

<!-- Check each item that applies. Explain any remaining unchecked items in Validation. -->

- [ ] I read [CONTRIBUTING.md](https://github.com/anatolek/helm-charts/blob/main/CONTRIBUTING.md) and follow the [code of conduct](https://github.com/anatolek/helm-charts/blob/main/CODE_OF_CONDUCT.md).
- [ ] The change is focused and links its related issue.
- [ ] For chart changes, affected examples render (`helm template`) and lint (`helm lint`).
- [ ] For chart documentation or values changes, generated READMEs (`helm-docs --skip-version-footer`) and the values schema (`helm schema --use-helm-docs`) are updated as applicable.
- [ ] For chart behavior changes, the chart version and `artifacthub.io/changes` annotation are updated.
- [ ] For community or documentation changes, links and any issue-form YAML are checked.
- [ ] Shared examples and logs contain no credentials or private information.
