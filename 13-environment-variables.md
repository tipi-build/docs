---
title: Environment variables
aliases: [ "06-environment-variables" ]
---



## Using a private tipi.build instance: `TIPI_ENDPOINT`

tipi.build can be run on premise or in a private deployment. All users of that deployment need to specify `TIPI_ENDPOINT` in their environment
to their `tipi` CLI to access the correct installation.

## Command line authentication

In non-interactive situation (a CI/CD job, other automated usages) it might be required to provide the `tipi` CLI with
credentials to access private repositories or make use of the tipi subscription.

- `TIPI_ACCESS_TOKEN` and `TIPI_REFRESH_TOKEN` are JWT tokens enabling `tipi` to get access to the tipi subscription.
- `TIPI_VAULT_PASSPHRASE` has to be supplied in situations where the user's Vault must be decrypted (ex. accessing private repositories)

## Customizing tools distribution `TIPI_DISTRO_JSON`

_Tipi_ uses a JSON file which contains the required tools used by tipi to build projects (like `cmake` or `make`). These tools are automatically downloaded and installed on demand by tipi at run-time before running projects build.

The contents of the file can be changed as per your needs for maximum usage flexibility setting the environment variable `TIPI_DISTRO_JSON`.

The environment variable may point to:

- an absolute or relative file path
- an HTTP(s) URL

The original JSON file can be found at https://github.com/tipi-build/distro/blob/master/distro.json

Below some examples of what you can set as `TIPI_DISTRO_JSON`:

```bash
export TIPI_DISTRO_JSON="~/projects/tipi/distro.json"
- or -
export TIPI_DISTRO_JSON="/home/user/projects/tipi/distro.json"
- or -
export TIPI_DISTRO_JSON="https://company.com/tipi/distro.json"
```

If `TIPI_DISTRO_JSON` is a HTTP(s) URL, tipi will download the file and check file integrity against the value in the environment variable `TIPI_DISTRO_JSON_SHA1`

## Customizing tools distribution `TIPI_DISTRO_JSON_SHA1`

When a customized `TIPI_DISTRO_JSON` is downloaded via HTTP(s) _tipi_ performs an integrity check by checking the `sha1sum` of the downloaded file against the value of `TIPI_DISTRO_JSON_SHA1`.

For example:
```bash
export TIPI_DISTRO_JSON="https://company/tipi/distro.json"
export TIPI_DISTRO_JSON_SHA1="4eb777d088ea949709e9ea97bbc8c389a63856e2"
```

## Distro installation mode `TIPI_DISTRO_MODE`

By default _tipi_ only installs the subset of the build tools required to build remotely using the tipi.build cloud.
For local builds you can install force the installation of the required tools locally.

For example:

```bash
export TIPI_DISTRO_MODE="all" # "full" install - takes ~7gb in TIPI_HOME_DIR 
 - or -
export TIPI_DISTRO_MODE="default" # "light" install for remote builds
```
