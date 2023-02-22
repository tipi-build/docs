---
title: Compile options
order: 4
---

# Compile options

## Passing `-D` defines

_Example:_

```bash
tipi . -DSOME_OPTION=1 -DOTHER_OPTION=OK
```

_Note:_ these definitions only affect the _local project_ and are not passed down to dependencies. If you need to set options of dependencies of your projects you can use the `opts` facilities in the `.tipi/deps` file [(see Dependencies and Project Configuration §`opts` : defines and compile-time options)](./02-dependencies#--opts--defines-and-compile-time-options)  or use the `<project-root>/.tipi/opts.toolchain` file instead.

## Compile options

_Tipi_ relies on the CMake project, which allows you to tweak the compilation flags even though _tipi_ typically sets sane defaults for you.

You may add your own toolchain files in `<tipi-home>/environments/<distro>` which is the preferred option.

If you specify compile options, they will be applied to all projects in the build tree in the context of the toolchain specific sysroot.

## `opts` files

You may specify project and target specific opts by creating `opts[.target-platform]` files in `<project-root>/.tipi/`.

The opts files have to contain valid CMake syntax. For example to pass #defines or compile options this way simply add:

```cmake
add_compile_options( -Wextra )
add_compile_definitions( DEFINE_TO_PASS_WITHOUT_D_BEFORE=1 )
```

_Note:_ these definitions only affect the _local project_ and are not passed down to dependencies. If you need to set options of dependencies of your projects you can use the `opts` facilities in the `.tipi/deps` file [(see Dependencies and Project Configuration §`opts` : defines and compile-time options)](./02-dependencies#--opts--defines-and-compile-time-options)  or use the `<project-root>/.tipi/opts.toolchain` file instead.

> If both a matching target-platform `.tipi/opts.target-platform` file *and* a non specific `.tipi/opts` file are defined the contents of both are injected into the build


## `opts.toolchain` file

If you need to inject some compiler setting in the CMake toolchain for your project and target and all its dependencies you may add a `opts.toolchain[.target-platform]` file(s) to your project.

As for the `.tipi/opts` files, this has to contain valid CMake systax.

_Note:_ setting `opts.toolchain` affects cache hits as it changes the ABI-hash of the whole project.

