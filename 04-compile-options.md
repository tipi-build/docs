---
title: Compile options
order: 5
---

# Compile options

## Passing `-D` defines

Example:

```bash
tipi . -DSOME_OPTION=1 -DOTHER_OPTION=OK
```

> The defines will transitively be passed to all your dependencies build as well.

## Compile options

_Tipi_ relies on the CMake project, which allows you to tweak the compilation flags even though _tipi_ typically sets sane defaults for you.

You may add your own toolchain files in `<tipi-home>/environments/<distro>` which is the preferred option.

If you specify compile options, they will be applied to all projects in the build tree in the context of the toolchain specific sysroot.

## `opts` file

You may specify project and target specific opts by creating `opts[.target-platform]` files in `<project-root>/.tipi/`.

The opts files have to contain valid CMake Syntax. For example to pass #defines or compile options this way simply add:

```cmake
add_compile_options( -fmath-errno -Wextra )
add_compile_definitions( DEFINE_TO_PASS_WITHOUT_D_BEFORE=1 )
```

> If both a matching target-platform `.tipi/opts.target-platform` file *and* a non specific `.tipi/opts` file are defined the contents of both are injected into the build
