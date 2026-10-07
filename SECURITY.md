# Security policy

## Reporting a vulnerability

Send suspected vulnerabilities privately to
[anatolek@ukr.net](mailto:anatolek@ukr.net). Use a subject such as
`[helm-charts security] Brief description`.

Do not disclose exploitable details in public issues, discussions, or pull requests
before coordinating with the maintainer. For ordinary bugs and usage questions, see
[SUPPORT.md](SUPPORT.md).

Include, when available:

- The affected `hlib` version and template or helper.
- Helm and Kubernetes versions relevant to the problem.
- A minimal, sanitized consumer chart, values, and commands that reproduce it.
- The affected rendered resources, security impact, and conditions needed to exploit it.
- Any suggested mitigation or fix.

Remove credentials, tokens, private keys, personal data, and real cluster secrets from
examples and logs. Reproduce the problem in an environment you are authorized to test.

## Versions and fixes

Reports may concern any release. Include the exact affected version and whether the
problem also occurs with the latest published release, if you can safely check.
The maintainer will assess fixes and any backports case by case. This policy does not
establish a long-term support schedule or guarantee fixes for historical versions.

The library generates Kubernetes resources used by consumer charts. Explain whether
the issue comes from library defaults, template behavior, or a specific consumer
configuration so its impact can be assessed.

## Coordinated disclosure

The maintainer and reporter should coordinate reproduction, mitigation, a fix, and
public disclosure. Response and release timing depend on the issue and maintainer
availability; no fixed response deadline is promised. If appropriate, published
security information will be available through the repository's
[security advisories](https://github.com/anatolek/helm-charts/security/advisories).
