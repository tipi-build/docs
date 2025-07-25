---
title: 💻 Environments
aliases: [ "00-environments", "04-environments" ]
---

A _CMake RE_ environment consist of 2 elements:

1. CMake toolchain file, as passed via `-DCMAKE_TOOLCHAIN_FILE=`
2. OS image specification in a `.pkr.js/` folder :
  - Dockerfile
  - Virtual Machine Image

The OS images are described by `pkr.js` folders and files to provide reproducible, containerized hermetic builds. 

They can be used on a local machine with `cmake-re` or on a remote build orchestrator ( _e.g._ tipi.build ).

## Generalization : Content Adressable Toolchains
Environments are passed via `-DCMAKE_TOOLCHAIN_FILE=` to `cmake-re`, but the real CMAKE_TOOLCHAIN_FILE used for the build will be substituted for another location **generalized** by `cmake-re`. 

Generalization in `cmake-re` means that any `-DCMAKE_TOOLCHAIN_FILE=<tolchain-name>.cmake` passed has it's parent directory transferred to a fixed, centralized and content-addressable location on every system, the path is based on the folder content.

The toolchain's fingerprint is then used as a (shareable) cache-key across projects. 

A **Generalized toolchain** is a toolchain and it's accompanying environment specification cleaned up of any developer system-specific elements. Essentially this guarantees that:
  1. Toolchains will always be located based on a Content Addressable location 
  2. File times won't affect the environment build process ( e.g. To guarantee cache use on OS Image / Docker creation )

### Configurability

Depending on how the environment descriptions and toolchains are stored changes to unrelated environments can affect the environment description hash that is computed during environment generalization because by default all files in the same directory than the referenced toolchain will be consumed.
In order to better compose and isolate environment specifications it is possible to configure the exact contents using [`<toolchain-name>.layers.json files`](./0410-environments-layering.md).

## OS Image File Lookup Rule

The environments specifications are pointed to for a build via the `cmake-re` `-DCMAKE_TOOLCHAIN_FILE=<toolchain>.cmake` command line parameter. 

OS Image are described as `.pkr.js/` folders. In order to determine the OS Image `cmake-re` looks for the **most specialized** OS Image it can find picking : 
1. The `<toolchain>.pkr.js` with the **exact toolchain name** as `<toolchain>.cmake`
2. The `*.pkr.js` file which **shares the most starting character** with `<toolchain>` in the same folder.
3. If this doesn't yield any results, it perfoms the same search in the default environments directory `/usr/local/share/.tipi/<distro>/environments/` (or `C:\.tipi\<distro>\environments\`).

### OS Image Lookup Example 

Given the following example set of toolchain files : 

```
└── environments
    ├── linux-cxx17.cmake
    ├── linux-cxx20.cmake
    ├── linux-cxx23.cmake
    ├── linux.pkr.js
    │   └── linux.pkr.js
    └── linux-cxx23.pkr.js
        ├── linux-cxx23.Dockerfile
        └── linux-cxx23.pkr.js
```

When `cmake-re` is provided `-DCMAKE_TOOLCHAIN_FILE=environments/linux-cxx23.cmake` the OS Image description `environments/linux-cxx23.pkr.js/` will be picked, while if `-DCMAKE_TOOLCHAIN_FILE=environments/linux-cxx17.cmake` or `-DCMAKE_TOOLCHAIN_FILE=environments/linux-cxx20.cmake` is provided the build will run within the environment described by `environments/linux.pkr.js/`.

Because in this example there are no `environments/linux-cxx23.layers.json` any change to `linux.pkr.js` would also affect rebuilding any code dependent `linux-cxx23.cmake`, if this isn't desired it's possible to add [`linux-cxx23.layers.json`](./0410-environments-layering.md) to isolate environments, while still giving possibility to compose common cmake modules.  

## Default Environments
Officially supported environments can be found in the [tipi-build/environments](https://github.com/tipi-build/environments) GitHub repository, they can be used as example for custom environments.

They are unpacked in the default environments directory `/usr/local/share/.tipi/<distro>/environments/` (or `C:\.tipi\<distro>\environments\`).

The `disto` ID key can be found by running `cmake-re --version` or `tipi --help`

```bash
> $ cmake-re --version
cmake-re v0.0.72 (distro id: de3f03a)
```

## Custom containerized environments

> **Requirement:** The environment has to contain a `cmake-re` remote rpc server
> This can be setup as in the following example.

You can use the following ready-made setup scripts to install the required components into mainstream distribution based container images: 

| System Family  | CMake RE remote server setup script                                                 |
|----------------|-------------------------------------------------------------------------------------|
| Centos, Redhat | https://raw.githubusercontent.com/tipi-build/cli/master/install/container/ubuntu.sh |
| Ubuntu, Debian | https://raw.githubusercontent.com/tipi-build/cli/master/install/container/centos.sh |

By default both scripts install only a minimal set of tools and put the `tipi` distribution into `default` mode which **does not** ship any compiler.

To revert to an installation mode more akin to the tipi desktop installation the following environment settings can be set before running the installer script:

- `TIPI_INSTALL_LEGACY_PACKAGES=ON` to install a variety of tools using the system package manager.
- `TIPI_DISTRO_MODE=all` to ship a full `tipi` / `cmake-re` distribution including the clang based default toolchain.

### Example

In the following let's create an custom build environment based on Ubuntu 24.04

```
└── environments
    ├── linux-cxx23.cmake (skipped here)
    └── linux.pkr.js
        ├── linux.Dockerfile
        └── linux.pkr.js
```

These scripts could be used as follow to make a build environment based on Ubuntu 24.04 :

#### `environments/linux.pkr.js/linux.Dockerfile`

>
> **NOTE:** This example environment doesn't contain any compiler. The custom environments need to provide the toolchains manually by installing the required tools. The official reusable environments can serve as-is or as examples : [tipi-build/environments](https://github.com/tipi-build/environments)

```Dockerfile
ARG UBUNTU_24_04="ubuntu@sha256:04f510bf1f2528604dc2ff46b517dbdbb85c262d62eacc4aa4d3629783036096"
FROM ${UBUNTU_24_04}

ARG DEBIAN_FRONTEND=noninteractive # avoid tzdata asking for configuration
# Install tipi and cmake-re
RUN apt update -y && apt install -y curl gettext
RUN curl -fsSL https://raw.githubusercontent.com/tipi-build/cli/master/install/container/ubuntu.sh -o ubuntu.sh && /bin/bash ubuntu.sh
USER tipi
WORKDIR /home/tipi
EXPOSE 22
```

The folder `environments/linux.pkr.js/` will be the used as docker build context and all files in this folder will be provided for the build of the container image.


#### `environments/linux.pkr.js/linux.pkr.js`

```js
{
  "variables": { },
  "builders": [
    {
      "type": "docker",
      "image": "tipibuild/testapp-cmake-re-containerized:latest",
      "commit": true
    }
  ],
  "post-processors": [
    { 
      "type": "docker-tag",
      "repository": "linux",
      "tag": "latest"
    }
  ]
}
```
> With this environment description `cmake-re` will pull the image `tipibuild/testapp-cmake-re-containerized:latest` and use it if it matches the linux.pkr.js/ content.
> Should it not exist or be *outdated* the `environments/linux.pkr.js/linux.Dockerfile` will be used to create or update it.

Please note that `cmake-re` will rely on the provided image to contain an installation matching the version of `cmake-re` used on the host system. Interoperability between different versions is not guaranteed. 

If required, `cmake-re` can inject version information into the environment description file (`linux.pkr.js` in this example) during the **environment generalization** process. This ties the environment description content hash to the version of `cmake-re` running the container image build. The preferred placeholder for this is `{{cmake_re_source_hash}}` which will be replaced by the exact source revision that was used to build the release of `cmake-re` (this is a git revision hash).

> The official tipi container images are using the `{{cmake_re_source_hash}}` placeholder in this way:
> ```json
> ...
> "image": "tipibuild/tipi-ubuntu:{{cmake_re_source_hash}}",
> ...
> ```

**Optionally** the `image` reference in `.pkr.js` environment description can be replaced with a *hard reference* (e.g. the docker image name with a repository manifest digest referencing one exact immutable image) to pin the image. In this case the environment description does not need to contain a Dockerfile.

#### Detection of image validity

`cmake-re` will add an image label to the generated image during the build of the container image to mark the container image with the specific environment description content hash (the value is computed before the container build and it is stored as `build.tipi.environment_fingerprint` build config label). This value will be checked prior to using the container to detect changes and to avoid *container name/tag-based* mismatches.

This means that a single character change to the environment specification (all files in the `<environment-name>.pkr.js/` folder) will trigger a rebuild of the container image even if an image with the same reference name exists in the container registry. This check can be bypassed by referencing the container with a *hard reference* (see note above).


