---
title: Ignore and exclude
aliases: [ "07-exclude-and-ignore", "08-ignore-and-exclude" ]
---

## Exclude and ignore using `.tipiignore`

You can create an ignore file named `.tipiignore` in the your project.
The syntax is identical to the syntax of `.gitignore` (official documentation for https://git-scm.com/docs/gitignore)

> Files that are ignored by your `.gitignore` are **not ignored** during tipi source scan

> Ignore rules can be provided as well via the `-x <ignorerule>` command line switch and `x` attribute in the [depspec](/documentation/02-dependencies)

### Target specific ignore rules using `.tipiignore.<target>`

If you find the need to specify target specific rules for your build (for example t exclude a target OS specific test or example from your otherwise
fully cross platform compatible library) you can create **target specific** `.tipiignore.<target>` files in your project.

> Please note that the `<target>` part requires an exact match (ex. for a `tipi . -t linux-cxx17` build you'd create a `.tipiignore.linux-cxx17` file).
> 
> Additionally the `.tipiignore` rules are considered a base rule set and are applied for all targets even when a specific match is found:
> this means that rules of  `.tipiignore.linux-cxx17` and of a base `.tipiignore` are *additive*  

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
