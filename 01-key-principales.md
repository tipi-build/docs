---
title: Key Principles and Goals
aliases: []
---

Tipi solves three of the most common problems C++ developers face day to day:

1. dependency management
2. long build times and
3. environments

by giving you:

- fetching dependencies straight from git repositories, no need to wait for package definitions
- speeding up your workflow with powerful multi-platform cloud environments
- shipping with a useful set of tools on Linux, MacOS, and Windows platforms


### A new C++ workflow

- Code scanning & conventions over build configuration
- Zero setup - just coding
    - select one environment from our [supported list](https://github.com/tipi-build/environments) or [specify your own](https://tipi.build/documentation/01-environments#customizing-environments)
    - tipi downloads & installs the compiler and libraries in an isolated distro folder automatically


### Environments on demand

We automatically provision repeatable build environments on powerful cloud build machines when you need them.
Learn more about how tipi environments are specified: [environment](/documentation/01-environments)


### Every project is a library

In a software project there are 2 kinds of entry-points:

- Developer entry-points for code reuse
- End-user entry-points for application use

By default tipi automatically builds both a library and an application (if something like a `main()` is found) from your sources to ease *reuse*.


### Don't pay for what you don't use

This is a core C++ design philosophy, and sadly the world of packages manager obliges you to take more than you need.

By definition a package does _pack_ of a lot of things, and the final application won't need all of them.

tipi allows you to do a fine-granular selection of your dependencies and pulls only the bits that are really required in your final application.

### Opinionated defaults (but you choose)

While tipi clearly is set out to enable you to *build anything* without complex scripts, we don't hold you back to customize parts (or all of) the build with `CMakeLists.txt.tpl` or `CMakeLists.txt` files associated with `use-cmake.tipi` files.



### tipi installation location ( former TIPI_HOME_DIR )

When launching `tipi` for the first time tipi[^1] will be installed at: 

  - On Windows: `C:\.tipi\`
  - On other platforms: `/usr/local/share/.tipi/`

_tipi_ will install dependencies, environment descriptions and tools for your environments in that location.

I case you want to specify an alternate location (if you don't have much space or no permission to write to that part of the disk) 
you should use the mechanisms of file-system junctions and bind mounts.

We guarantee the paths even in non-containerized builds to enable caching of artifacts. 

[^1]: by using `tipi run` to launch the binary you make sure your OS as has all the required libraries in its search path, for ex. the `libstdc++6` on windows.
