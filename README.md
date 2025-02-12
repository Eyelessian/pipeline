# GitHub Actions for Okteto Cloud

## Automate your development workflows using Github Actions and Okteto Cloud
GitHub Actions gives you the flexibility to build an automated software development workflows. With GitHub Actions for Okteto Cloud you can create workflows to build, deploy and update your applications in [Okteto Cloud](https://cloud.okteto.com).

Get started today with a [free Okteto Cloud account](https://cloud.okteto.com)!

# Github Action for developers to trigger Okteto Pipelines from a GitHub Actions workflow

You can use this action to trigger an [Okteto Pipeline](https://okteto.com/blog/cloud-based-development-environments/) based on Github events.

You can use this action to enable your CI/CD workflow in [Okteto Cloud](https://cloud.okteto.com).

[This document](https://okteto.com/docs/tutorials/getting-started-with-pipelines/index.html) has more information on this workflow.

## Inputs

### `name`

The name of the pipeline.

### `namespace`

The Okteto namespace to use. If not specified it will use the namespace specified by the `namespace` action.

### `timeout`

The length of time to wait for completion. Values should contain a corresponding time unit e.g. 1s, 2m, 3h. If not specified it will use `5m`.

### `skipIfExists`

Skip the pipeline deployment if the pipeline already exists in the namespace (defaults to false)

### `variables`

A list of variables to be used by the pipeline. If several variables are present, they should be separated by commas e.g. VAR1=VAL1,VAR2=VAL2,VAR3=VAL3.

## Environment variables

### `CUSTOM_CERTIFICATE`

The self-signed certificate of your environment. Best set on global level when using multiple Okteto actions.

# Example usage

This example runs the login action, activates a namespace, and triggers the Okteto pipeline

```yaml
# File: .github/workflows/workflow.yml
on:
  pull_request:
    branches:
      - master

name: example

jobs:
  devflow:
    runs-on: ubuntu-latest
    steps:
    - name: checkout
      uses: actions/checkout@master

    - name: Login
      uses: okteto/login@master
      with:
        token: ${{ secrets.OKTETO_TOKEN }}

    - name: "Activate Namespace"
      uses: okteto/namespace@master
      with:
        name: cindylopez

    - name: "Trigger the pipeline"
      uses: okteto/pipeline@master
      with:
        name: pr-${{ github.event.number }}
        timeout: 8m
        skipIfExists: true
        variables: "DB_HOST=mysql,CONFIG_PATH=/tmp/config.yaml"
```

