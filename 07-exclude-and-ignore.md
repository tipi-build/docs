---
title: Ignore and exclude
order: 8
---

# Exclude and ignore using `.tipiignore`

You can create an ignore file named `.tipiignore` in the root of your project.
The syntax is identical to the syntax of `.gitignore` (official documentation for https://git-scm.com/docs/gitignore)

> Files that are ignored by your `.gitignore` are **not ignored** during tipi source scan

> Ignore rules can be provided as well via the `-x <ignorerule>` command line switch and `x` attribute in the [depspec](/documentation/02-dependencies)

#### Examples

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
