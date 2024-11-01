---
title: 💻 Environment Layers Specifications
aliases: [ ]
---

Environments can be built from multiple layers and the [**generalization process**](./0400-environments.md) can be configured to maximize toolchain composition and cache reusability.

They are inspired by the concept of Bitbake layers, their main reason of existence is to permit cmake toolchain code reuse across similar toolchains, but avoid unnecessary rebuilds or cache invalidation between toolchains layer files. 

## Example 
A telling example would be in the case of 2 toolchains building for linux once, with clang and once with gcc.

It's important that build caches get invalidated when generic settings are modified like the one in `fpic.cmake` which represents POSITION_INDEPENDENT_CODE ( often necessary when linking bigger C++ binaries ) but it would be unfortunate to have a change to gcc specific options invalidate the build of clang.

Without layer files exactly this would happen in the following example : 
```
└── environments
    ├── flags
    │   └── fpic.cmake
    ├── linux-cxx23-gcc.cmake
    ├── linux-cxx23-clang.cmake
    └── linux-cxx23.pkr.js
        ├── linux-cxx23.Dockerfile
        └── linux-cxx23.pkr.js
```

Adding a layer file for each toolchain allow to limit which parts of the environments will be generalized and isolate environments from changes that aren't wanted : 

### `environments/linux-cxx23-gcc.layers.json`
```json
[
    "linux-cxx23-gcc.cmake",
    "flags/*.cmake"
]
```
### `environments/linux-cxx23-clang.layers.json`
```json
[
    "linux-cxx23-clang.cmake",
    "flags/*.cmake"
]
```

The layers file allow specifying getting files from parent directories and the above example could be simplied to a single layer files only taking the flags for each independent ultimate toolchain file.


## `<toolchain>.layers.json` Syntax
Add a `<toolchain-name>.layers.json` file next to the `<toolchain-name>.cmake` file.

```json
[
    "<globbing rule>",
    ...
]
```

- `?` single char which is not a directory separator
- `*` any number of chars which are not a directory separator
- `**` match everything at any nesting
- `../` go up one level .... *obviously*
- Exclude rules can be written by having them start with `!` (e.g. `!**/.gitignore` will exclude any `.gitignore` file under the *search base folder*)

Some notable things:
- the *search base folder* is the parent folder of `<toolchain-name>.cmake`
- if one (or more) `../` folder navigations happen, the *virtual root folder* will be moved up by the corresponding paths / the nesting will be replicated as necessary
- the rules **have to be** relative to *search base folder*
- rules are overridden by order so inclusions / exclusions can be combined quite flexibly

