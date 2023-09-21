---
title: Build by convention
aliases: [ "01-conventions" ]
---

## Why ?

Why did we ever write Makefile, CMakeLists.txt or configure IDE project settings? And why are we still doing it ?

Andrew Koenig once coined FTSE, the [Fundamental theorem of software engineering](https://en.wikipedia.org/wiki/Fundamental_theorem_of_software_engineering): 

> **"We can solve any problem by introducing an extra level of indirection."**

Building applications is an extremely complex problem, and the layers are almost infinite: linker, assembler, compiler, front-end, build-system ( *I.e.* `make` ), meta-build-system ( *I.e.* `cmake` ) to quote only a few.

_Tipi_ leverages all the fabulous work done in these layer to finally make building C++ a humane task.

## Because

- C++ code expresses explicitly enough what is an application entry-point and what is reusable library code.
- There's no need to learn a new language to specify how to build.
- _We are tired of specifying each single file that should be built._

## Convention by example

Take the idea of building a game project. This game project will contain: 

1. the game domain with the player characters, game menu and so on
2. the game app itself
3. tools apps like game maps and texture editor

The directory could look like this

<div class="columns">
  <div class="column is-10">
    <content-img-figure src="./assets/build-by-convention-00.png">
      game directory tree as in tipi demo
    </content-img-figure>
  </div>
</div>

`game.cpp` has a main function each, therefore they are *apps* by convention.

The main function may look like: 

<div class="columns">
  <div class="column is-10">
    <content-img-figure src="./assets/build-by-convention-01.png">
      game main function
    </content-img-figure>
  </div>
</div>

During the can of `src/` _tipi_ classifies the `*.hpp` and `*.cpp` which do not have any entry-point to the library code ( *i.e.* the game domain).

In `tools/` `map_editor.cpp` and `texture_editor.cpp` are found to both have `main()` functions, which has the **apps** convention kick-in.

The **apps** convention allows to have supporting file residing locally, therefore `common.cpp is` linked to `map_editor` and `texture_editor`. The header `common.hpp` is accessible via `#include "common.hpp"` or `#include <common.hpp>` while the classes in `src/game_classes/*` are exported on the compiler include directories which makes them usable via `#include <game_classes/*.hpp>`.

tipi will give the following summary: 

<div class="columns">
  <div class="column is-10">
    <content-img-figure src="./assets/build-by-convention-02.png">
      build summary
    </content-img-figure>
  </div>
</div>

Resulting in a bin folder like the following:

```
  <target>/bin
  ├── game.exe                // game.cpp
  ├── demo.lib                // game_classes *.cpp
  └── tools
      ├── map_editor.exe      // common.cpp, map_editor.cpp
      └── texture_editor.exe  // common.cpp, texture_editor.cpp
```

### Conventions Types

Main conventions:

- **every project is a library**. ( *ref:* [every project is a library](/documentation/#every-project-is-a-library))
- each git repositories passed to `tipi` results in one library

> All the conventions described are applied if required and/or possible based on tipi's smart detection
>
> It is possible to specify or reduce the scope of a library within a project in case the detection fails or hinting is required:
>
> - by adding parameters: `tipi -s library-dir`
> - by adding dependency descriptors in the `.tipi/deps` file: `"dependency/repo": { "s" : [ "library-dir"" ] }`
>
> ...with `-s` (or the `s` key) specifying the additional search path for _source_ files. Multiple entries can be specified 
> both a multiple `-s` parameters or multiple entries in the `s` key.

#### Split libraries

```
  .
  ├── include
  │   └── header.hpp
  └── src
      └── impl.cpp
```

One typical kind of C++ project is when headers and implementation files are split in different folders. _tipi_ will consider the `include/` folder to be the publicly installable headers and `src/` folder to contain the implementation files constituting the library.

> _tipi_ checks the presence of `include/`, `inc/`, `src/` and `sources/` to infer this convention.

#### Same directory libraries

```
  .
  └── src
      ├── header.hpp
      └── impl.cpp
```

Another typical convention C++ programmers use is having implementation and headers residing at the same level of the project's directory hierarchy.

> _tipi_ checks the presence of `src/` and `sources/` to infer this convention and the absence of main() functions in the files.

#### Top-level libraries

These are libraries that don't have any special source folder, their headers are directly rooted at the top of their repositories.

When this is detected the same mechanism as for **Same directory libraries** applies. 

> In this kind of structure disambiguation it might be required to to tell which directories are part of the lib using the `tipi -s` switch or the matching `.tipi/deps` configuration.

#### Header-only libraries

<!-- ::::TODO rewrite this part:::: -->

It is possible to have code which is completely header only while application entry-points are in .cpp files aside materializing either lib **examples** or a corresponding **app**.

The headers will be put at disposal like in the aforementioned conventions with ``#include <>``.

#### apps

Any .cpp file with an entry-point is considered to be an app.

For example any file containing a `main()` function or a macro instantiating a `main()` function (as commonly used in unit testing frameworks) will be compiled as an application.

> apps are always linked to the project library.
>
> if others .cpp files reside at same or deeper file-system directories they get linked with the applications in question. Exception to this rule are those directories explicitly declared in the `s`/ `-s` configuration.

#### tests or examples

Equivalent to the **apps** convention, however they will registered within the CMake CTest test driver and calling ``tipi . --test all`` will run them all and report result status.

This convention kicks-in when files with `main()` functions in parent folder are named after one of `test`, `tests`, `example`, `examples`

#### HTML

Any `.html` file containing ``<script type="text/c++"></script>`` in it is compiled using the **app** convention.

### When conventions are not enough

In some cases you may find that the convention based build doesn't suit your needs or fails on certain edge-cases.

In such a case please reach out using our [community support forum](https://github.com/tipi-build/community-support) or your premium support option.

Alternatively you can also try to tweak the build as explained in the chapters below. We'd really love to hear about your issues with the smart build algorithm still in order to improve _tipi_ and find a way for the convention build to work, because we firmly believe that _less CMakeLists is more time for your C++_.

#### Override convention build for specific directories

Add an empty marker file `use-cmake.tipi` and a valid `CMakeLists.txt` to the folder you want to exclude from the convention based build.

This can be useful for custom test framework or in cases you need to use specific CMake features not yet supported by tipi.

#### Tweaking convention build

_Tipi_ relies on CMake and the way me use it can be customized by adding `CMakeLists.txt.tpl` files in your project.

The `CMakeLists.txt.tpl` can be placed anywhere in the project and are applied to the matching sub-tree.

To generate a sample CMakeLists.txt.tpl with the docs embedded of the different variables at your disposal run `tipi cmaketpl` in any project folder.
