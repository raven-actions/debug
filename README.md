# 🩺 Dump Context Action

[![GitHub - marketplace](https://img.shields.io/badge/marketplace-dump--context--action-blue?logo=github&style=flat-square)](https://github.com/marketplace/actions/dump-context-action)
[![GitHub - release](https://img.shields.io/github/v/release/DariuszPorowski/dump-context-action?style=flat-square)](https://github.com/DariuszPorowski/dump-context-action/releases/latest)
[![GitHub - ci](https://img.shields.io/github/actions/workflow/status/DariuszPorowski/dump-context-action/ci.yml?logo=github&label=CI&style=flat-square&branch=main&event=push)](https://github.com/DariuszPorowski/dump-context-action/actions/workflows/ci.yml?query=branch%3Amain+event%3Apush)
[![GitHub - license](https://img.shields.io/github/license/DariuszPorowski/dump-context-action?style=flat-square)](https://github.com/DariuszPorowski/dump-context-action/blob/main/LICENSE)

This action allows you to quickly and easily dump [GitHub contexts](https://docs.github.com/en/actions/learn-github-actions/contexts) and Runner environment, giving you a comprehensive overview of the current state of your GitHub workflow. This can be useful for GitHub workflows development, troubleshooting, debugging, or understanding how different components interact with each other.

- Action is platform-independent.
- The output of each dump is in the `JSON` format as much as possible.

![demo](https://raw.githubusercontent.com/DariuszPorowski/dump-context-action/main/assets/images/demo.png)

## Table of Contents <!-- omit in toc -->

- [🤔 Usage](#-usage)
  - [Quick Start](#quick-start)
  - [Extra context](#extra-context)
  - [Run only when `GitHub debug logging` enabled](#run-only-when-github-debug-logging-enabled)
- [📥 Inputs](#-inputs)
- [👥 Contributing](#-contributing)
- [📄 License](#-license)

## 🤔 Usage

### Quick Start

Just place in your GitHub workflow steps:

```yaml
- name: Dump Context
  uses: DariuszPorowski/dump-context-action@v1
```

### Extra context

By default, [composite](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action) action cannot access contexts like `vars`, `secrets` `needs`, `inputs`. So if you want to include them in the dump, you must specify them explicitly. For more details, follow [📥 Inputs](#-inputs) section.

```yaml
- name: Dump Context
  uses: DariuszPorowski/dump-context-action@v1
  with:
    vars-context: ${{ toJson(vars) }}  # optional
    secrets-context: ${{ toJson(secrets) }}  # optional
    needs-context: ${{ toJson(needs) }}  # optional
    inputs-context: ${{ toJson(inputs) }}  # optional
```

### Run only when `GitHub debug logging` enabled

In certain circumstances (e.g., re-run failing workflow), if you'd like to run `Dump Context Action` only when [GitHub debug logging](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/enabling-debug-logging) is enabled, then set `env` on workflow or job level:

```yaml
env:
  DEBUG: ${{ secrets.ACTIONS_RUNNER_DEBUG || vars.ACTIONS_RUNNER_DEBUG || secrets.ACTIONS_STEP_DEBUG || vars.ACTIONS_STEP_DEBUG || false }}
```

and add `if: ${{ env.DEBUG }}` condition to the action.

```yaml
- name: Dump Context
  if: ${{ env.DEBUG }}
  uses: DariuszPorowski/dump-context-action@v1
  with:
    vars-context: ${{ toJson(vars) }}  # optional
    secrets-context: ${{ toJson(secrets) }}  # optional
    needs-context: ${{ toJson(needs) }}  # optional
    inputs-context: ${{ toJson(inputs) }}  # optional
```

## 📥 Inputs

| Name            | Required | Type          | Default value | Description                                                                                               |
|-----------------|----------|---------------|---------------|-----------------------------------------------------------------------------------------------------------|
| vars-context    | false    | `json object` | *not set*     | The context for the `vars` must be explicitly provided, as it is unavailable in the composite actions.    |
| secrets-context | false    | `json object` | *not set*     | The context for the `secrets` must be explicitly provided, as it is unavailable in the composite actions. |
| needs-context   | false    | `json object` | *not set*     | The context for the `needs` must be explicitly provided, as it is unavailable in the composite actions.   |
| inputs-context  | false    | `json object` | *not set*     | The context for the `inputs` must be explicitly provided, as it is unavailable in the composite actions.  |

## 👥 Contributing

Contributions to the project are welcome! Please follow [Contributing Guide](https://github.com/DariuszPorowski/dump-context-action/blob/main/.github/CONTRIBUTING.md).

## 📄 License

This project is distributed under the terms of the [MIT](https://github.com/DariuszPorowski/dump-context-action/blob/main/LICENSE) license.
