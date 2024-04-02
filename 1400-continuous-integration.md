---
title: Continuous integration
aliases: [ "05-continuous-integration" ]
---

## With GitHub Actions 

The tipi subscription can be used to run your build and test your project in GitHub actions.

Run `tipi ci` to generate the YAML configuration for GitHub CI.

### Authenticating on tipi.build from CI workflows
Information is available as reference here : [Command Line authentication](13-environment-variables#command-line-authentication) to handle authentication in CI workflows.

An example `.github/workflows/ci.yaml` would look like the following : 

```yaml
name: build 
# This workflow is triggered on pushes to the repository.
on: [push]

env: 
  TIPI_ACCESS_TOKEN: "${{ secrets.TIPI_ACCESS_TOKEN }}"
  TIPI_REFRESH_TOKEN: "${{ secrets.TIPI_REFRESH_TOKEN }}"
  TIPI_VAULT_PASSPHRASE: ${{ secrets.TIPI_VAULT_PASSPHRASE }}

jobs:
  build: 
    name: build-linux
    runs-on: ubuntu-latest
    container: tipibuild/tipi-ubuntu
    steps:
      - name: checkout
        uses: actions/checkout@v2
      - name: tipi builds project 
        run: |
          tipi connect
          tipi build . --target linux --dont-upgrade --verbose --test all 
```
