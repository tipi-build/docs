---
title: Environments
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
Environments are passed via `-DCMAKE_TOOLCHAIN_FILE=` to `cmake-re`, but the real CMAKE_TOOLCHAIN_FILE used for the build will be substitutd to another location **generalized** by `cmake-re`. 

Generalization in `cmake-re` means that any `-DCMAKE_TOOLCHAIN_FILE=<tolchain-name>.cmake` passed get it's parent directory transferred in a fixed centralized content-addressable location on every system, the path is based on the folder content.

If the folder content is binary equal the toolchains enables toolchains to be used as a shareable cache-key across projects. 

A Generalized toolchains is a toolchain and it's accompanying environment specification cleaned up of any developer system-specific elements, essentially this guarantees that :
  1. Toolchains will always be located based on a Content Addressable location 
  2. File times won't affect the environment build process ( e.g. To guarantee cache use on OS Image / Docker creation )

### Configurability
Because generalization essentially picks all files in the toolchain directory, while not necessary, it's possible to configure it with [`<toolchain-name>.layers.json files`](./0410-environments-layering.md) to better compose and isolate toolchains from each other sitting in the same folder.


## OS Image File Lookup Rule
The environments specifications are pointed to for a build via the `cmake-re` `-DCMAKE_TOOLCHAIN_FILE=<toolchain>.cmake` command line parameter. 

OS Image are described as `.pkr.js/` folders, in order to determine within which OS Image the build will be run, `cmake-re` looks for the **most specialized** OS Image it can find picking : 
1. The `<toolchain>.pkr.js` with the **exact same name** than `<toolchain>.cmake`
2. The next `.pkr.js` file which **shares the most starting character** with `<toolchain>` in the same folder.
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
Officially supported environments can be found in the [tipi-build/environments](https://github.com/tipi-build/environments) GitHub repository.

They are unpacked in the default environments directory `/usr/local/share/.tipi/<distro>/environments/` (or `C:\.tipi\<distro>\environments\`).

## Custom Environments

> **Requirement:** The environment shall contain a `cmake-re` remote rpc server
> This can be setup as in the following example.

To get the minimum required to bootstrap an hermetic build, a ready-made setup script for your environment can be setup for use with CMake RE : 

| System Family  | CMake RE remote server setup script                                                 |
|----------------|-------------------------------------------------------------------------------------|
| Centos, Redhat | https://raw.githubusercontent.com/tipi-build/cli/master/install/container/ubuntu.sh |
| Ubuntu, Debian | https://raw.githubusercontent.com/tipi-build/cli/master/install/container/centos.sh |

### Custom Docker Environment Example
These scripts could be used as follow to make a build environment based on Ubuntu 22.04 : 

#### `environments/linux.pkr.js/linux.Dockerfile`
```Dockerfile
FROM ubuntu:22.04
ENV TIPI_DISTRO_MODE=all

ARG DEBIAN_FRONTEND=noninteractive # avoid tzdata asking for configuration
# Install needed tools
RUN apt update -y && apt install -y curl
RUN curl -fsSL https://raw.githubusercontent.com/tipi-build/cli/master/install/container/ubuntu.sh -o ubuntu.sh && /bin/bash ubuntu.sh
EXPOSE 22
```


#### `environments/linux.pkr.js/linux.pkr.js`

>
> **NOTE:** `cmake-re` will pull the image `tipibuild/testapp-cmake-re-containerized` and use it if it exists, if it doesn't exists or is outdated the `environments/linux.pkr.js/linux.Dockerfile` will be used to create or update it.
>

```js
{
  "variables": { },
  "builders": [
    {
      "type": "docker",
      "image": "tipibuild/testapp-cmake-re-containerized:{{tipi_cli_version}}",
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
  ,"_tipi_version":"{{tipi_version_hash}}"
}
```


