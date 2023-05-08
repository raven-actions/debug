# 🩺 Debug Action

[![GitHub - marketplace](https://img.shields.io/badge/marketplace-debug--github--action-blue?logo=github&style=flat-square)](https://github.com/marketplace/actions/debug)
[![GitHub - release](https://img.shields.io/github/v/release/raven-actions/debug?style=flat-square)](https://github.com/raven-actions/debug/releases/latest)
[![GitHub - ci](https://img.shields.io/github/actions/workflow/status/raven-actions/debug/ci.yml?logo=github&label=CI&style=flat-square&branch=main&event=push)](https://github.com/raven-actions/debug/actions/workflows/ci.yml?query=branch%3Amain+event%3Apush)
[![GitHub - license](https://img.shields.io/github/license/raven-actions/debug?style=flat-square)](https://github.com/raven-actions/debug/blob/main/LICENSE)

This [GitHub Action](https://github.com/features/actions) allows you to quickly and easily dump [GitHub contexts](https://docs.github.com/en/actions/learn-github-actions/contexts) and Runner environment and post action webhook payloads to the [SMEE.io](https://smee.io) service, giving you a comprehensive overview of the current state of your GitHub workflow. This can be useful for developing, troubleshooting, debugging GitHub workflows or actions, or understanding how different components interact.

- Action is platform-independent and tested on all the latest GitHub-hosted runners (`ubuntu-latest`, `macos-latest`, `windows-latest`).
- The output of each dump is in the `JSON` format as much as possible.

![demo](https://raw.githubusercontent.com/raven-actions/debug/main/assets/images/demo.png)

## Table of Contents <!-- omit in toc -->

- [🤔 Usage](#-usage)
  - [Quick Start](#quick-start)
  - [Extra contexts](#extra-contexts)
  - [Run only when `GitHub debug logging` enabled](#run-only-when-github-debug-logging-enabled)
  - [SMEE.io](#smeeio)
- [📥 Inputs](#-inputs)
- [📤 Outputs](#-outputs)
- [👥 Contributing](#-contributing)
- [📄 License](#-license)

## 🤔 Usage

### Quick Start

Just place in your GitHub workflow steps:

```yaml
- name: Debug
  uses: raven-actions/debug@v1
```

### Extra contexts

By default, [composite](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action) action cannot access contexts like `vars`, `secrets` `needs`, `inputs`. So if you want to include them in the dump, you must specify them explicitly. For more details, follow [📥 Inputs](#-inputs) section.

> ⚠️ `secrets` contexts will not show your secrets in the log! It's masked `***` by default.

```yaml
- name: Debug
  uses: raven-actions/debug@v1
  with:
    vars-context: ${{ toJson(vars) }}  # optional
    secrets-context: ${{ toJson(secrets) }}  # optional
    needs-context: ${{ toJson(needs) }}  # optional
    inputs-context: ${{ toJson(inputs) }}  # optional
```

### Run only when `GitHub debug logging` enabled

In certain circumstances (e.g., re-run failing workflow), if you'd like to run `Debug Action` only when [GitHub debug logging](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/enabling-debug-logging) is enabled, then set `env` on workflow or job level:

```yaml
env:
  DEBUG: ${{ secrets.ACTIONS_RUNNER_DEBUG || vars.ACTIONS_RUNNER_DEBUG || secrets.ACTIONS_STEP_DEBUG || vars.ACTIONS_STEP_DEBUG || false }}
```

and add `if: ${{ env.DEBUG == 'true' }}` condition to the action.

```yaml
- name: Debug
  if: ${{ env.DEBUG == 'true' }}
  uses: raven-actions/debug@v1
  with:
    vars-context: ${{ toJson(vars) }}  # optional
    secrets-context: ${{ toJson(secrets) }}  # optional
    needs-context: ${{ toJson(needs) }}  # optional
    inputs-context: ${{ toJson(inputs) }}  # optional
```

### SMEE.io

> ⚠️ This feature is intended for debugging purposes only and should not be used to handle **sensitive data**. Additionally, please be aware that any authentication does not secure SMEE.io, and anyone with the channel ID can access your payloads.

Aid in debugging GitHub Action runs by expertly posting webhook payloads to [SMEE.io](https://smee.io).

To access the channel, please make sure you have a browser tab open to the channel URL.

```yaml
- name: Debug
  id: debug
  uses: raven-actions/debug@v1
  with:
    smee: true  # optional, if not set then default is `false`
    smee-channel: my-custom-channel-name # optional, if not set then default is `repositoryOwner-repositoryName`, e.g. raven-actions-debug

- name: Debug outputs
  if: ${{ steps.debug.outputs.smee == 'true' }}  # example usage
  run: |
    echo "Your SMEE.io URL is: ${{ steps.debug.outputs.smee-url }}"
```

## 📥 Inputs

|       Name        | Required |     Type      | Default value | Description                                                                                               |
|:-----------------:|:--------:|:-------------:|:-------------:|-----------------------------------------------------------------------------------------------------------|
|  `vars-context`   |  false   | `json object` |   *not set*   | The context for the `vars` must be explicitly provided, as it is unavailable in the composite actions.    |
| `secrets-context` |  false   | `json object` |   *not set*   | The context for the `secrets` must be explicitly provided, as it is unavailable in the composite actions. |
|  `needs-context`  |  false   | `json object` |   *not set*   | The context for the `needs` must be explicitly provided, as it is unavailable in the composite actions.   |
| `inputs-context`  |  false   | `json object` |   *not set*   | The context for the `inputs` must be explicitly provided, as it is unavailable in the composite actions.  |
|      `smee`       |  false   |    `bool`     |    `false`    | Aid in debugging GitHub Action runs by expertly posting webhook payloads to SMEE.io.                      |
|  `smee-channel`   |  false   |   `string`    |   *not set*   | If the SMEE channel ID is not provided, the ID of the repository will be utilized instead.                |

## 📤 Outputs

|    Name    |   Type   | Description                                                 |
|:----------:|:--------:|-------------------------------------------------------------|
| `smee-url` | `string` | The SMEE URL to use. If SMEE is not used, it will be empty. |

## 👥 Contributing

Contributions to the project are welcome! Please follow [Contributing Guide](https://github.com/raven-actions/debug/blob/main/.github/CONTRIBUTING.md).

## 📄 License

This project is distributed under the terms of the [MIT](https://github.com/raven-actions/debug/blob/main/LICENSE) license.
