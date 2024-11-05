---
title: 👩🏼‍💻 cmake-re --help | Command Line Reference
---

**CMake RE :** CMake Remote Execution, transparent cmake wrapper with build isolation and caching capabilities.

If you know to use `cmake`, using `cmake-re` is a drop-in replacement which runs cached and hermetic builds by default.

The key differences to plain `cmake` are : 

  * [`--remote`](#hermetic---remote---host) Builds remotely in an isolated + hermetic environment
  * [`--host`](#hermetic---remote---host) Disable local containerized build. Builds locally on this host without isolation or hermeticity but with caching.
  * [`--monitor|-m`](#--monitor-m) Monitors source tree, rebuilding on every changes.

## `cmake-re` : Configure
Configures and Generate the build system. By default CMake RE relies on Ninja
```
cmake-re [<<path-to-source>>]
  [-S <<path-to-source>]
  [-B <<path-to-build-dir>>] 

  -DCMAKE_TOOLCHAIN_FILE=<toolchain-file>

  [-D<<var>[:<type>]=<value>>]
  [-C <<path-to-initial-CMakeCache.txt>>] 

  [-G <generator>] 
  [--install-prefix <<path-to-install-dir>>]

  [--remote]
  [--host]
  [-j|--jobs <cpus>]   
```

### hermetic, `--remote`, `--host`
Not specifying any of these options launches a containerized build by default.
* `--remote`                Builds remotely in an isolated + hermetic environment.
* `--host`                  Disable local containerized build. Builds locally on this host without isolation or hermeticity but with caching.
* `-j, --jobs <cpus>`       How many CPU cores have to be dedicated to the build.

### Mandatory
* `-S <<path-to-source>`    Path to directory with the `CMakeLists.txt` file of the CMake RE project to build.
* `-B <<path-to-build-dir>>` Path to directory which CMake RE will use as the root of build directory.
* `-DCMAKE_TOOLCHAIN_FILE=<toolchain-file>` specifies on and for which [environment](./0400-environments.md) to build 

### Optional
* `-D<<var>[:<type>]=<value>>` Create or update a CMakeCache.txt variable entry.
* `-C <<path-to-initial-CMakeCache.txt>>` Pre-load a script to populate the cache.
* `-G <generator>`          Specify a build system generator. Defaults to Ninja ( CMake RE cache-capable fork of ninja )
* `--install-prefix <<path-to-install-dir>>` Specify the installation directory. Must be an absolute path. `[CMAKE_INSTALL_PREFIX]`
  


### `--monitor, -m`
* `--monitor, -m`           Monitors source tree, rebuilding on every changes, reconfiguring as needed.

 
## `--build <<build-dir>>` : Build
Build the CMake Project.

```
[ 
  --build {
      <<build-dir>> 
    [--target|-t <<target>>] 
    [--config <<config>>] 
    [--clean-first] 
      [
        -- <build-tool-options>... // Ninja
      ] 
  }
] 
```

* `--target, -t <<tgt>>` Build `<tgt>` instead of default targets.
* `--config <<cfg>>`     For multi-configuration tools, choose `<cfg>`.
* `--clean-first`           Build target 'clean' first, then build.
* `-- <build-tool-options>...` Arguments to forward unchanged to the build tool


## `--run-test` : Test
Control test executions, run tests on each build together with `--monitor`

```
[ 
  --build {
      <<build-dir>> 

    [--run-test <<test-target>>] 
    [--exclude-tests <<revision-hash>>] 
    [--test-jobs <N>] 
  }
] 
```
* `--run-test <<test-target>>` CTest to execute
* `--exclude-tests <<revision-hash>>` regular expression to match test to exclude. When --run-test=all is passed, this allows excluding a test or more.
* `--test-jobs <N>`         How many test executables will be run concurrently.

## `--cache`
**CMake RE** caches configuration and build phase automatically, it's possible to manually control cache-id keys and the behaviour.
Manually perform cache populate and restore operation, this is useful for integration in external package manager and packaging or release pipelines.

```
  [ 
    --cache { <<operation>> --revision <<revision-hash>> }
  ] 
  [--origin <<cache-origin>>]
  [--only-mirror]
  [-n|--no-refresh]
```

* `--cache <<operation>>`
  * `--cache populate` : Use the current build and install tree as cache to populate for the given `--revision`
  * `--cache restore` : Search in local and remote cache for the given `--revision`  and restor in `-B` and `--install-prefix`
  * `--revision <<revision-hash>>` Revision to restore/populate
* `--origin <<cache-origin>>` Override repository origin to address cache entries ( cache-id ). Useful on forks to get upstream cache.
* `--only-mirror`           Do not configure or build, only mirror sources and create required build tree and indirection in mirror.
* `-n, --no-refresh`        Run fully offline, use only offline data, don't update from remote cache.


## `--help`
Simply this page.
```
  [-?|-h|--help]
  [--version|-V]
  [-v|--verbose] 
```

* `-?, -h, --help`
* `--version, -V`           Print version information
* `-v, --verbose`           verbosity level. Can be repeated to trace all e.g. `-vv`.
