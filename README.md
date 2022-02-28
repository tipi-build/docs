---
title: Intro and getting started
order: 0
---

# Introduction

Tipi solves three of the most common problems C++ developper face day to day: dependency management, long build times and environments:

- speed up your workflow with powerful multiplatform cloud environments
- fetch dependencies straight from git repositories, no need to wait for package definitions
- shipped with a useful set of tools on three platforms


## Getting started

1. create your account on [tipi.build](https://tipi.build/)
2. install `tipi` :
    - Tipi Visual Studio Code extension: &nbsp; [<img src="~/assets/vscode.png" style="height: 1em; vertical-align: middle;">&nbsp; Add to Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=tipi.tipi-build)
    - Command line util:

```bash
# Linux & MacOS:
/bin/bash -c \
 "$(curl -fsSL https://raw.githubusercontent.com/tipi-build/cli/master/install/install_for_macos_linux.sh)"
```

```powershell
# Windows 10 / 11 in Powershell
[Net.ServicePointManager]::SecurityProtocol = "Tls, Tls11, Tls12, Ssl3"
. { `
  iwr -useb https://raw.githubusercontent.com/tipi-build/cli/master/install/install_for_windows.ps1 `
} | iex

# P.S.: we highly recommend you give a the new Windows Terminal app a try. It truly augments your 
# console experience on Windows!
```

3. run `tipi connect` and link your installation to your [tipi.build](https://tipi.build/) account so you can use your tipi subscription
4. create an empty folder for the example project on your disk
5. create an `example.cpp` and write a simple hello world:

```cpp
#include <iostream>

int main(int argc, char** argv) {
  std::cout << "tipi is cool ! " << std::endl;
  return 0;
}
```

> ##### Note on platforms and environments:
>
> You can replace occurences of `linux-cxx17` in the instructions below with `windows` or `macos-cxx17` when using your subscription
> or the environment matching your machine's platform
>
> On Linux: `linux-cxx17` or `linux-cxx20`
>
> On MacOS: `macos-cxx17` or `macos-cxx20`
>
> On Windows: `windows` or `windows-cxx17` or `windows-cxx20` or `vs-16-2019-cxx17` (if you have Build Tools for Visual Studio 2019 installed)
>
>
> When using your tipi subscription to build or run, a cloud node of the corresponding platform is deployed in the tipi cloud.

6. build the example using either:
    - your [tipi subscription](https://tipi.build/dashboard/subscription): `tipi build . -t linux-cxx17`
    - your local machine: `tipi . -t linux-cxx17`

7. run the resulting binary using:
    - your [tipi subscription](https://tipi.build/dashboard/subscription): `tipi .run /build/linux-cxx17/bin/example -t linux-cxx17`
    - your local machine: `tipi run /build/linux-cxx17/bin/example` [^1]

8. Add a dependency from Github: we're going to add a json manipulation library from: [github.com/nlohmann/json](https://github.com/nlohmann/json)
    - create a file `.tipi/deps` with content

```json
{
	"nlohmann/json" : { "@" : "v3.10.4" }
}
```
  
> Note: we are pinning the version of the dependency to the tagger release `v3.10.4` (list can be found under 
> [nlohmann/json/releases](https://github.com/nlohmann/json/releases) ). At time of building tipi will
> pull the release from the github repository and build it. If you want to live on the edge, you can remove
> the `@` pin or write a branch name like `master` in there.

9. Edit your `example.cpp`:

```cpp
#include <iostream>
#include <nlohmann/json.hpp> // tipi.build will find it online

int main(int argc, char** argv) {

  std::cout << "Wonderful JSON formatter with tipi is cool ! " << std::endl;

  auto json = nlohmann::json::parse(argv[1]);
  std::cout << json.dump(2) << std::endl;
  return 0;
}
```

10. Compile and run (see #6 and #7 above):

```bash
$> tipi run ./build/linux-cxx17/bin/example '[4,52,25]'
Wonderful JSON formatter with tipi is cool ! 
[
  4,
  52,
  25
]
```

## Key principles and goals

### Finally: the C++ flow

- Code scanning & conventions over build configuration
- 0 setup - just coding
    - select one environment from our [supported list](https://github.com/tipi-build/environments) or [specify your own](https://tipi.build/documentation/00-environments#customizing-environments)
    - tipi downloads & installs the compiler and libraries in an isolated distro folder automatically

### Environments on demand

We automatically provision repeatable build environments on powerful cloud build machines when you need them.
Learn more about how tipi environments are specified: [environment](/documentation/00-environments)

### Every project is a library

In a software project there are 2 kinds of entrypoints:

- Developer entrypoints for code reuse
- End-user entrypoints for application use

By default tipi automatically builds both a library and an application (if something like a `main()` is found) from your sources to ease reuse.

### Don't pay for what you don't use

This is a core C++ design philosophy, and sadly the world of packages manager obliges you to take more than you need.

By definition a package does _pack_ of alot of things, and the final application won't need all of them.

tipi allows you to do a fine-granular selection of your dependencies and pulls only the bits that are really required in your final application.

### Opinionated defaults (but you choose)

While tipi clearly is set out to enable you to _build anything_ without complex scripts, we don't hold you back to customize parts (or all of) the build with `CMakeLists.txt.tpl` or `CMakeLists.txt` files associated with `use-cmake.tipi` files.

[^1]: by using `tipi run` to launch the binary you make sure your OS as has all the required libraries in its search path, for ex. the `libstdc++6` on windows.
