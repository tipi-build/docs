---
title: Environment variables
aliases: [ "06-environment-variables" ]
---



## Using a private `cmake-re` deployment instance: `TIPI_ENDPOINT` &amp; `RBE_service`

`cmake-re` can be run on a private cloud deployment or on-prem. All users of that deployment need to specify `TIPI_ENDPOINT` &amp; `RBE_service` in their environment, so that the `cmake-re` and `tipi` CLI access the correct deployment.

The syntax for these environment variables are : 
* `TIPI_ENDPOINT=https://<deployment-address>` ( without ending `/` )
* `RBE_service=<cluster-address>:<port>`

## Command line authentication

In non-interactive situation (a CI/CD job, other automated usages) it might be required to provide the `tipi` CLI with
credentials to access private repositories or make use of the tipi subscription.

### tipi vault authentication
- `TIPI_ACCESS_TOKEN` and `TIPI_REFRESH_TOKEN` are JWT tokens enabling `tipi` to get access to the tipi subscription.
- `TIPI_VAULT_PASSPHRASE` has to be supplied in situations where the user's Vault must be decrypted (ex. accessing private repositories)

### Distributed build authentication
- `RBE_tls_client_auth_key` : User specific mTLS authentication private key to RBE_service
- `RBE_tls_client_auth_cert` : User specific mTLS authentication public certificate to RBE_service

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

## Tipi container build local registry port `TIPI_LOCAL_REGISTRY_PORT`

> In order to provide a consistent cache keying for containerized (both local and remote) and distributed tipi and cmake-re generate an environment description 
> that contains a hard reference to the container that will be used to execute the build.
>
> This process requires that locally built images (read: a `Dockerfile` is provided as part of the environment specification and there is no matching and valid 
> image available on the registry) be pushed to a registry to generate that hard reference data if the docker runtime of the host system is not using an OCI
> compliant internal storage for the images (read: most Docker installations on Linux at time of writing).
>
> To automate this process `tipi` and `cmake-re` will start an ephemeral local container registry and push to that as needed to generate the required data.

By default the ephemeral local container registry will be hosted on `127.0.0.1:43113` 

The port can be changed by setting the environment variable `TIPI_LOCAL_REGISTRY_PORT` to any integer value in the range [1024 - 65535].

## Passing additional parameters to the environment build command `TIPI_CONTAINER_BUILD_ADDITIONAL_PARAMETERS`

Additional parameters (docker buildx arguments) can be passed to the environment builder by setting the environment variable `TIPI_CONTAINER_BUILD_ADDITIONAL_PARAMETERS` 
before running a build that will determine that a container image needs rebuilding.

This can be useful if one needs to set a build tag or image label in a CI setup that is making environment images available to developers:

```bash
export TIPI_CONTAINER_BUILD_ADDITIONAL_PARAMETERS="--label org.myself.note=hello --tag localref123:latest"
```

After running the build of the container image the image build configuration will contain the `org.myself.note` label and the image will be available as `localref123:latest` on the host.

> ⚠️ Note: changes to this parameters will not be taken into account during the computation of cache keys or the like nor will they trigger a re-build of an otherwise unchanged environment.

## Source map rewriting in compiler output `TIPI_SOURCE_MAP`

By default, `tipi` and `cmake-re` rewrite the paths in the compiler output. Error messages and warnings point to the local files of the main project instead of referencing the internal copies for better ease of use and IDE integrations. To disable this behavior, you can set this environment variable to `OFF`. The default value is `ON`.

```bash
export TIPI_SOURCE_MAP="OFF"
```

## Tipi home directory TIPI_HOME_DIR

By default, tipi and cmake-re install their tools, environments, and build artifacts under a platform-specific user directory.
Setting the environment variable TIPI_HOME_DIR allows you to fully control where tipi and cmake-re store their data.
When TIPI_HOME_DIR is set, it is used as the root directory for:
- Installed tools
- Hermetic environments and toolchains
- Source checkouts used for hermetic builds
- Build outputs and intermediate artifacts generated by tipi and cmake-re

```bash
export TIPI_HOME_DIR="/tmp/.tipi/"
```

> ⚠️ Note: Changing TIPI_HOME_DIR effectively changes the identity of the build environment.
Tools, caches, and environments installed under a previous location will not be reused unless the same path is configured again.