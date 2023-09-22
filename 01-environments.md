---
title: Environments
aliases: [ "00-environments" ]
---

A _tipi.build_ environment consist of 2 elements:

- CMake tool-chain file
- OS image

The OS images are described via the help of Packer files and can be automatically deployed on the tipi.build cloud as many-core build agent or as execution environment for unit test.

Officially supported environments can be found as `<environment>.pkr.js` + `<environment>.cmake` file pairs in the [tipi-build/environments](https://github.com/tipi-build/environments) GitHub repository.

While emphasizes on providing a default environment for each major platform, community maintained or private environments are welcome.

As a good starting point tipi always provides at clean and up-to-date clang with `libc++` STL for following environments:

- WebAssembly: `tipi -t wasm build . `
- Linux:  `tipi -t linux build . `
- Windows: `tipi -t windows build . `
- MacOS: `tipi -t macos build . `

Variations specifying C++ standard versions are also available (adding the suffix `-cxx17` or `-cxx20`).

> hint: The `.` (dot) in the commands above can be any path containing C++ source files or a C++ CMake project.

## Building in the tipi.build cloud

```bash
tipi  -t linux-cxx17 build .
```

Will provision an environment as described in `/usr/local/share/.tipi/<distro>/environments/linux.pkr.js` (or `C:\.tipi\<distro>\environments\linux.pkr.js`) on the tipi.build and run the build using your tipi subscription.

All required files will be synchronized bidirectionally to and from the tipi build node as necessary.

## Building on your local machine

```bash
tipi -t linux-cxx17 .
```

When your machine fits your target environment you can also use the tipi provided tool-chain to build locally.

## Running apps on remote environments

**Note:** Make use of your subscription to run your application on the remote tipi node.

Run the binary built from the `./app.cpp` source file:

```bash
tipi -t macos-cxx17 .run build/bin/app
```

> Notes:
>
> - all parameters after `.run` are forwarded to the remote environment
> - the standard IO is redirected to the calling console

## Running within the tipi environment for your local machine

```bash
tipi run build/bin/app
```

Will run a command within the tipi environment on your local machine. Note the absence of `.` in front of `run` vs. `.run` from the previos example.

## Customizing environments

Tipi environments can be customized to your need by creating the required set of CMake tool-chain and Packer file.

Examples can be found at [tipi-build/environments](https://github.com/tipi-build/environments) .

Once they are stored in `/usr/local/share/.tipi/<distro>/environments/<environment>.pkr.js` and `/usr/local/share/.tipi/<distro>/environments/<environment>.cmake` a `tipi build . -t <environment>` will start the image creation the tipi cloud and then build the current sources.

[^1]: to have an environment definition curated, please submit a pull request to [tipi-build/environments](https://github.com/tipi-build/environments) on GitHub. Tipi will then take care of having the images maintained and deployment ready at all time.
