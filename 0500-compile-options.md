---
title: Compile options
aliases: [ "05-compile-options" ]
---

Specify the compiletime environment with CLI arguments or store in the `opts` files.

## Changing build parallelity
By default tipi figures out the best parallelity for the current machine you are using based on the available RAM and CPU counts available.
Use the `-j` parameter to specify another build parallelity (number of cores to use).


```bash
tipi . -t linux-cxx17 -j 8
```

_The `-j 8` parameter sets the build parallelity to 8 cores/threads_


```bash
tipi build . -t linux-cxx17 -j 128
```

_The `-j128` parameter sets the build parallelity to 128 cores/threads and selects the appropriately sized remote machine in the tipi build cloud_

Please note that when using this parameter in conjunction with remote builds it becomes part of the target specification. This means that specifying, meaning that targeting `-t linux-cxx17 -j8` targets a different machine than targetting `-t linux-cxx17 -j32`



## Passing `-D` defines constants

_Example:_

```bash
tipi -t linux . -DSOME_OPTION=1 -DOTHER_OPTION=OK
```

**Note:** these definitions only affect the _local project_ and are not passed down to dependencies. 

### Defining constants for remote builds

Constants are defined per project in the `<project-root>/.tipi/deps` file [(see Dependencies and Project Configuration §`opts` : defines and compile-time options)](./02-dependencies#--opts--defines-and-compile-time-options) with the `opts` member or using the `<project-root>/.tipi/opts.toolchain` file instead.

<!-- TODO what happens if one does both? -->



## Compile options

_Tipi_ relies on the CMake project, which allows you to tweak the compilation flags even though _tipi_ typically sets sane defaults for you.

You may add your own tool-chain files in `<tipi-home>/environments/<distro>` which is the preferred option.

If you specify compile options, they will be applied to all projects in the build tree in the context of the tool-chain and target specific build directory aka `sysroot`.



## `opts` files

You may specify project and target specific opts by creating `opts[.target-platform]` files in `<project-root>/.tipi/`.

The opts files have to contain valid CMake syntax. For example to pass #defines or compile options this way simply add:

```cmake
add_compile_options( -Wextra )
add_compile_definitions( DEFINE_TO_PASS_WITHOUT_D_BEFORE=1 )
```

**Note:** these definitions only affect the _local project_ and are not passed down to dependencies. 
If you need to set options of dependencies of your projects you can use the `opts` facilities in the `.tipi/deps` file [(see Dependencies and Project Configuration §`opts` : defines and compile-time options)](./02-dependencies#--opts--defines-and-compile-time-options)  or use the `<project-root>/.tipi/opts.toolchain` file instead.

> If both a matching target-platform `.tipi/opts.target-platform` file *and* a non specific `.tipi/opts` file are defined the contents of both are injected into the build


## `opts.toolchain` file

If you need to inject some compiler setting in the CMake tool-chain for your project and target and all its dependencies you may add a `opts.toolchain[.target-platform]` file(s) to your project.

As for the `.tipi/opts` files, this has to contain valid CMake syntax.

_Note:_ setting `opts.toolchain` affects cache hits as it changes the ABI-hash of the whole project.

