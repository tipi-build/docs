---
title: 🚀 Getting started with CMake RE for C++
aliases: [ "00-getting-started" ]
---

In this getting started guide we will setup your machine to perform local CMake containerized hermetic and cached builds.

1. Install `cmake-re` a drop in replacement for CMake :

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

```

> ##### Prerequisites for Hermetic Builds
>
> For local containerized and hermetic build you need to **install docker** on your system, if you can't install it you can rely on `--remote` and a tipi.build account to run the container on our cloud. Or resort to cached but non-hermetic builds with `--host`.
>
> ➡ [Docker Engine Installation Guide](https://docs.docker.com/engine/install/) 
>
> ❗️**Docker Engine 26.0.0** or newer required : Check your installed version with `docker version`
> 

2. [Download our get-started example](https://github.com/tipi-build/get-started) `CMakeLists.txt` and a CMake RE environment decription ( `CMAKE_TOOLCHAIN_FILE` ) : 
     - `git clone https://github.com/tipi-build/get-started.git`

The **get-started** example presents a CMake Project depending on the [fmtlib](https://fmt.dev) that will be cached by CMake RE `FetchContent` support, making FetchContent a full fledged package manager ( Read more about our [Compatible and Enhanced FetchContent](https://github.com/tipi-build/cmake-tipi-provider) ).

> ##### Content of a CMake RE project
> A CMake RE project is a plain CMake project with the addition of environment descriptions to guarantee the build reproducibility and hermeticity.
> 
> This allows CMake builds to always get a well-defined environment for reproducible builds, making **build caching and remoting** possible.
> 
> Environment Descriptions are made of :
>   - * A `CMAKE_TOOLCHAIN_FILE`, _e.g._ `environment/linux.cmake`
>   - * An accompanying .pkr.js and Dockerfile, _e.g._ `environments/linux.pkr.js/`, `environments/linux.pkr.js/linux.Dockerfile`
>
> Multiple CMAKE_TOOLCHAIN_FILE with longer names than .pkr.js environments descriptions can share the same base system, more details in [Environments](/documentation/0400-environments).
> 
> Environments are not limited to docker containers, Virtual Machine images are also usable.
> 
> ```
> ├── CMakeLists.txt
> ├── environments
> │   ├── linux.cmake
> │   └── linux.pkr.js
> │       ├── linux.Dockerfile
> │       └── linux.pkr.js
> └── test
>     └── main.cpp
> ```
> 


#### `CMakeLists.txt`
```cmake
cmake_minimum_required(VERSION 3.27.6)

project(get-started)
enable_testing()

# Install fmt
Include(FetchContent)
set(FMT_TEST OFF CACHE BOOL "" FORCE)
FetchContent_Declare(fmt
  GIT_REPOSITORY https://github.com/fmtlib/fmt.git
  GIT_TAG        9.0.0
  OVERRIDE_FIND_PACKAGE
)
FetchContent_MakeAvailable(fmt)
find_package(fmt REQUIRED)

# Add a test executable
add_executable(main test/main.cpp)
target_link_libraries(main PRIVATE fmt::fmt)

add_test(NAME main COMMAND ${CMAKE_CROSSCOMPILING_EMULATOR} $<TARGET_FILE:main> )
```

#### `test/main.cpp`
```cpp
#include <fmt/core.h>

int main() {
    fmt::print("Good Morning Caching & Hermetic Builds :-) \n"); 
    return 0;
}
```

#### `environments/linux.pkr.js/linux.pkr.js`
```js
{
  "variables": { },
  "builders": [
    {
      "type": "docker",
      "image": "tipibuild/tipi-ubuntu:{{tipi_cli_version}}", // The Docker to use
      "commit": true                                         // or update when Dockerfile present
    }
  ],
  "post-processors": [
    { 
      "type": "docker-tag",
      "repository": "linux",
      "tag": "latest"
    }
  ]
  ,"_tipi_version":"{{tipi_version_hash}}"


}
```
Aside of the `CMAKE_TOOLCHAIN_FILE` the `pkr.js` folder specifies the environment that will be used for the build.
This is a [packer docker builder configuration](https://developer.hashicorp.com/packer/integrations/hashicorp/docker/latest/components/builder/docker), **different to plain packer** `cmake-re` will build the Docker if it cannot be found on a registry or changed when a `linux.Dockerfile` exists inside the `pkr.js` folder.

In this case we just take the default linux tipi environment with a full fledged clang toolchain. 

More details in [Environments](/documentation/0400-environments)

3. Cached, Reproducible Hermetic CMake builds :

```bash
$> cmake-re -S . -B build/ -DCMAKE_TOOLCHAIN_FILE=environments/linux.cmake
```

This will launch the container on your machine and execute the build inside it, while caching the build execution.

You can get the build to rerun on any changes and check your test outputs with `--monitor` and `--run-test all`, alternatively if you don't want to run the containerized build but still use caching on your bare metal host, you can use : `--host`.