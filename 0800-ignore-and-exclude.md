---
title: Ignore and exclude - Caching and Mirroring
aliases: [ "07-exclude-and-ignore", "08-ignore-and-exclude" ]
---
As explained in [L1 Build Cache](./0360-build-cache.md) `cmake-re` mirrors code in an invariant path in the hermetic environment for the build, this mirroring can be configured as follow. 

## `.gitignore`
Mirroring doesn't overwrite .gitignored files in **source mirror** by default, this behaviour is to support in-source-tree generated files during the build.
It happens that projects `.gitignore` in-source-tree generated files when files are not checked-in. 

> Files that are ignored by your `.gitignore` present in your **original** source tree are **not ignored** during source code mirroring.
The behaviour can be customized by implementing inverse `.tipiignore` rules.


## `.tipiignore`
You can create an ignore file named `.tipiignore` in the your project.
The syntax is identical to the syntax of `.gitignore` (official documentation for https://git-scm.com/docs/gitignore)

### Target specific ignore rules using `.tipiignore.<toolchain-name>`

If you find the need to specify target specific rules for your build (for example to exclude a target OS specific test or example from your otherwise
fully cross-platform compatible library) you can create **toolchain specific** `.tipiignore.<toolchain-name>` files in your project.

> Please note that the `<toolchain-name>` part requires an exact match to the toolchain.
>
> Additionally the `.tipiignore` rules are considered a base rule set and are applied for all toolchains even when a specific match is found:
> this means that rules of  `.tipiignore.linux-cxx17` and of `.tipiignore` are *additive*

### Examples

```gitignore
# exclude everything except directory foo/bar
/*
!/foo
/foo/*
!/foo/bar
```

```gitignore
# exclude every .cpp ending by a number or by .swp.
[0-9].cpp
*.swp.cpp
```
