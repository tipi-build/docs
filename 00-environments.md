---
title: Environments
order: 1
---

## Environments

A _tipi.build_ environment consits of 2 elements:

- CMake toolchain file
- OS image

The OS images are described via the help of Packer files and can be automatically deployed on the tipi.build cloud as many-core build agent or as execution environment for unit test.

Officially supported environments can be found as `<environment>.pkr.js` + `<environment>.cmake` file pairs in the [tipi-build/environments](https://github.com/tipi-build/environments) github repository.

While emphasizes on providing a default environment for each major platform, community maintained or private environments are welcome.

As a good starting point tipi always provides at clean and up-to-date clang with libc++ STL for following environments:

- Webassembly: `tipi build . -t wasm`
- Linux:  `tipi build . -t linux`
- Windows: `tipi build . -t windows`
- macOS: `tipi build . -t macos`

Variations specifiying C++ standard versions are also available (adding the suffix `-cxx17` or `-cxx20`).

> hint: The `.` (dot) in the commands above can be any path containing C++ source files or a C++ CMake project.

## Building in the tipi.build cloud

```bash
tipi build . -t linux-cxx17 
```

Will provision an environment as described in `/usr/local/share/.tipi/<distro>/environments/linux.pkr.js` (or `C:\.tipi\<distro>\environments\linux.pkr.js`) on the tipi.build and run the build using your tipi subscription.

All required files will be synchronized bidirectionally to and from the tipi build node as necessary.

## Building on your local machine

```bash
tipi . -t linux-cxx17
```

When your machine fits your target environment you can also use the tipi provided toolchain to build locally.

## Running apps on remote environments

Make use of your subscription to run your application on the remote tipi node:

```bash
tipi -t macos-cxx17 .run build/bin/app
```

> Notes:
>
> - all paramters after `.run` are forwarded to the remote environment
> - the standard IO is redirected to the calling console

## Running within the tipi environment for your local machine

```bash
tipi run build/bin/app
```

Will run a command within the tipi environment on your local machine. Note the abscence of `.` in front of `run` vs.

## Customizing environments

Tipi environments can be customized to your need by creating the required set of CMake toolchain and Packer file.

Examples can be found at [tipi-build/environments](https://github.com/tipi-build/environments) .

Once they are stored in `/usr/local/share/.tipi/<distro>/environments/<environment>.pkr.js` and `/usr/local/share/.tipi/<distro>/environments/<environment>.cmake` a `tipi build . -t <environment>` will start the image creation the tipi cloud and then build the current sources.

Image creation and storage are billed on your subscription. Non-currated images[^1] are stored for 14 days after the last usage before being evicted from cache. Tipi provides consulting services for the creation of custom images is required, please contact us for more information.

[^1]: to have an environment definition currated, please submit a pull request to [tipi-build/environments](https://github.com/tipi-build/environments) on Github. Tipi will then take care of having the images maintained and deployment ready at all time.
