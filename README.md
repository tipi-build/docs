---
title: Intro and getting started
aliases: []
---

# Introduction

Tipi solves three of the most common problems C++ developers face day to day:

1. dependency management
2. long build times and
3. environments

by giving you:

- fetching dependencies straight from git repositories, no need to wait for package definitions
- speeding up your workflow with powerful multi-platform cloud environments
- shipping with a useful set of tools on Linux, MacOS, and Windows platforms


## Getting started

1. create your account on [tipi.build](https://tipi.build/)
2. install `tipi` :
    - Tipi Visual Studio Code extension: &nbsp; [<img src="~/assets/vscode.png" style="height: 1em; vertical-align: middle;">&nbsp; Add to Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=tipi.tipi-build)
    - Command line utility:

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
4. create an empty folder for the example project on your disk and `cd` into it
5. create an `example.cpp` inside that folder and write a simple hello world:

```cpp
#include <iostream>

int main(int argc, char** argv) {
  std::cout << "tipi is cool!" << std::endl;
  return 0;
}
```

> ##### Note on platforms and environments:
>
> You can replace occurrences of `linux-cxx17` in the instructions below with `windows` or `macos-cxx17` when using your subscription
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
    - your [tipi subscription](https://tipi.build/dashboard/subscription): `tipi -t linux-cxx20 build . `
    - your local machine[^1]: `tipi -t linux-cxx20 . `

7. run the resulting binary using:
    - your [tipi subscription](https://tipi.build/dashboard/subscription): `tipi -t linux-cxx20 .run build/linux-cxx20/bin/example`
    - your local machine[^1]: `tipi run build/linux-cxx20/bin/example`

8. Add a dependency from GitHub: we're going to add a JSON manipulation library from: [github.com/nlohmann/json](https://github.com/nlohmann/json)
    - create a file `.tipi/deps` with content

```json
{
	"nlohmann/json" : {
		"@" : "v3.11.2",
		"u": false,
		"x": [
			"benchmarks",
			"/tests",
			"/docs",
			"/tools"
		]
	}
}

```
  
> Note: we are pinning the version of the dependency to the tagger release `v3.11.2` (list can be found under 
> [nlohmann/json/releases](https://github.com/nlohmann/json/releases) ). At time of building tipi will
> pull the release from the GitHub repository and build it. If you want to live on the edge, you can remove
> the `@` pin or write a branch name like `master` in there.

9. Edit your `example.cpp`:

```cpp
#include <iostream>
#include <nlohmann/json.hpp> // tipi.build will find it online

int main(int argc, char** argv) {

  std::cout << "Wonderful JSON formatter with tipi is cool!" << std::endl;

  auto json = nlohmann::json::parse(argv[1]);
  std::cout << json.dump(2) << std::endl;
  return 0;
}
```

10. Compile and run (see #6 and #7 above):

```bash
$> tipi -t linux-cxx20 build .
$> tipi -t linux-cxx20 .run ./build/linux-cxx20/bin/example '[4,52,25]'
Wonderful JSON formatter with tipi is cool!
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
    - select one environment from our [supported list](https://github.com/tipi-build/environments) or [specify your own](https://tipi.build/documentation/01-environments#customizing-environments)
    - tipi downloads & installs the compiler and libraries in an isolated distro folder automatically


    ### Environments on demand

We automatically provision repeatable build environments on powerful cloud build machines when you need them.
Learn more about how tipi environments are specified: [environment](/documentation/01-environments)


### Every project is a library

In a software project there are 2 kinds of entry-points:

- Developer entry-points for code reuse
- End-user entry-points for application use

By default tipi automatically builds both a library and an application (if something like a `main()` is found) from your sources to ease *reuse*.


### Don't pay for what you don't use

This is a core C++ design philosophy, and sadly the world of packages manager obliges you to take more than you need.

By definition a package does _pack_ of a lot of things, and the final application won't need all of them.

tipi allows you to do a fine-granular selection of your dependencies and pulls only the bits that are really required in your final application.

### Opinionated defaults (but you choose)

While tipi clearly is set out to enable you to *build anything* without complex scripts, we don't hold you back to customize parts (or all of) the build with `CMakeLists.txt.tpl` or `CMakeLists.txt` files associated with `use-cmake.tipi` files.



### tipi installation location ( former TIPI_HOME_DIR )

When launching `tipi` for the first time tipi[^1] will be installed at: 

  - On Windows: `C:\.tipi\`
  - On other platforms: `/usr/local/share/.tipi/`

_tipi_ will install dependencies, environment descriptions and tools for your environments in that location.

I case you want to specify an alternate location (if you don't have much space or no permission to write to that part of the disk) 
you should use the mechanisms of file-system junctions and bind mounts.

We guarantee the paths even in non-containerized builds to enable caching of artifacts. 

[^1]: by using `tipi run` to launch the binary you make sure your OS as has all the required libraries in its search path, for ex. the `libstdc++6` on windows.
