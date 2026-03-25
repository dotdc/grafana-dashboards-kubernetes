# Contributing Guidelines

Please note that this project is released with a [Contributor Code of Conduct](CODE_OF_CONDUCT.md). By participating in this project you agree to abide by its terms.

## Project Scope

This project aims to offer a set of modern Grafana dashboards for Kubernetes.\
Because setups could be very diverse, it is not possible to make these dashboards universal.\
Changes are welcome as soon as they work with [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack).

## How to Contribute

- **Submit an issue :** to report a bug, share an idea or open a discussion.
- **Submit a pull request:** to share a fix or a feature.

## Best practices

- Bump dashboard(s) version(s)
- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) is preferred
- [Signed commits](https://git-scm.com/docs/git-commit#Documentation/git-commit.txt---signoff) is preferred

## AI-assisted contributions

Contributions generated or significantly assisted by AI tools (LLMs, coding agents, etc.) are welcome, but must be clearly identified to help maintainers review them appropriately.

- **Commits:** use the `🤖` scope in your conventional commit message, e.g.:

```text
feat(🤖): add memory usage panel to k8s-views-pods
fix(🤖): correct CPU throttling query
```

- **Pull requests:** prefix the PR title with `[🤖]`, e.g.:

```text
[🤖] feat: add network I/O breakdown panel
```

AI-generated content is held to the same quality standards as any other contribution. Maintainers may ask for clarification or request changes, just as with any PR.
