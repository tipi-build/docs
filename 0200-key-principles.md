---
title: Key Principles and Goals
aliases: [ "02-key-principales" ]
---

CMake RE helps tackling the biggest challenges in native development :

- Build reproducibility for CMake
- Long build times
- Dependencies management

by giving developers:

- Hermeticity & Reproducibility for CMake
- Build Remoting and Distribution
- enhanced CMake `FetchContent` for smart dependencies fetching and build caching: no need to bother with packages
- Powerful cross-platform parallel build and test environments in the cloud or self-hosted
- Build toolchain fully included and extensible for Linux, macOS, and Windows or custom platforms

### Full compatibility & Full Flexibility

One of the overarching design thoughts of CMake RE is to augment every developer's CMake experience while keeping a functional plain `cmake` workflow.

The `cmake-re` command is a compatible drop-in replacement for `cmake`, enabling **hermetic** and **cached** local and remote builds.

Wether the builds is run on a public cloud, on a private self-hosted deployment in containers or on bare-metal hosts this is the developer's choice.

### Build from sources without paying the cost

- Automatic CMake build caching connected to git
- Zero setup - just coding
    - select one environment from our [supported list](https://github.com/tipi-build/environments) or [specify your own](https://tipi.build/documentation/01-environments#customizing-environments)
    - tipi downloads & installs the compiler and libraries in an isolated distro folder automatically


### Environments on demand

We automatically provision repeatable build environments on powerful cloud build machines when you need them.
Learn more about how tipi environments are specified: [environment](/documentation/04-environments)


### CMakeLists.txt generation on demand 
If you have a codebase for which you don't have any CMakeLists.txt and would like to integrate it in your CMake project codebase, cmake-re is distributed with a tool named `tipi`.
The tool can among many things be used to generate CMakeLists automatically based on source code scanning.
See [🔮 EXPERIMENTAL - CMakeLists.txt Generator](./1700-build-by-conventions.md)


### Source Code Mirroring

When launching `tipi` for the first time tipi will setup it's home dir at:

  - On Windows: `C:\.tipi\`
  - On other platforms: `/usr/local/share/.tipi/`

_tipi_ will  install dependencies, environment descriptions and tools for your environments in that location.

It will also mirror any source you build there, and use path rewriting in compiler and test outputs to make it transparent to you. This mechanism allows for invariant paths in cache binaries, profiling data which makes build and library cache reuse even possible on bare-metal hosts. You can read more about this mechanism in the section about cmake-re [L1 Build Cache](./0360-build-cache.md).

I case you want to specify an alternate location (if you don't have much space or no permission to write to that part of the disk)
you should use the mechanisms of file-system junctions and bind mounts.

With this [`cmake-re` guarantees the paths even in non-containerized builds](/documentation/10-tipi-cache) to enable reuse of cached builds artifacts anywhere.
