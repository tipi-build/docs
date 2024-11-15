---
title: Accessing hermetic builds folders
aliases: [ "11-downloading-build-results" ]
---

## Local hermetic build
All containers builds are stored on the host filesystem. Which means they are directly accessible :
  * `cmake-re`: In the folder pointed by `-B <build-folder>` 
  * `tipi` : In `build/<toolchain-name` 

`cmake-re` creates indirections with symlink to provide access to it, they are located in `$HOME/.tipi/containers-workdirs`.

These are named after the container that were used for the build `$HOME/.tipi/containers-workdirs/<container-name>-<env-hash>`.

### Rationale
*docker* engine doesn't support mounting container volumes from the host, but support binding hosts filesystem into container, therefore instead of relying on our build folder synchronization mechanism used in remote context, we followed the **zero cost abstraction** C++ mantra to provide maximum performance where possible.

The choice for the location and the symlinks indirection is based on leveraging default VirtioFS file sync on macOS platform for *docker* containers, while working for Windows and Linux at the same time.

This choice maximizes build speed by using native IO performance without the overhead of docker's *overlayfs* while limiting copying from and to containers whenever possible.

## `--host` build
They are on your `--host`, symlinked from the **invariant** build cache location.
  * `cmake-re`: In the folder pointed by `-B <build-folder>` 
  * `tipi` : In `build/<toolchain-name>` 

## `--remote` build
Starting with `tipi` and `cmake-re` `v0.0.51` you can download build results directly onto your machine without downloading the full build directory.


- Download a single file `tipi -t linux-cxx17 download "./build/linux-cxx17/bin/src/string_to_json"`
- Download multiple files with a wildcard `tipi -t linux-cxx17 download './build/linux-cxx17/bin/src/string*'`
> Note the single quote to avoid local shell expansion, so that all remote files get downloaded.

- To synchronize the full build tree after the remote build append **`--sync-build`** to the build command line : 
  - `tipi -t linux-cxx17 -u build . --sync-build -- -DCMAKE_TOOLCHAIN_FILE=environments/linux.cmake`