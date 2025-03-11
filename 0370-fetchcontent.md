---
title: 📦 FetchContent() and Package Managers
aliases: [ "09-shared-cache", "1100-shared-cache" ]
---

## `FetchContent()` as Package Manager
`cmake-re` [**L1 Build Cache**](./0360-build-cache.md) is a git connected automatic build cache for CMake, it can be used as a C++ package manager or as ⚡ batteries to supercharge your favorite package manager.

One of the built-in CMake package manager that is used broadly is the `FetchContent()` module. `FetchContent()` is rudimentary and simple and hence slow when starting a fresh build on a CI node or during developer onboarding. The **cmake-tipi-provider** allows to express any dependencies with the best CMake Package Manager out there **FetchContent**.

**Advantages :**
  - Without changes `FetchContent` is automatically cached.
  - `cmake-re` automatically injects the cmake-tipi-provider. 
  - `CMakeLists.txt` stays fully compatible to plain CMake without `tipi` or `cmake-re`
 
## `HermeticFetchContent` : CMake FetchContent with Hermeticity, SBOMs and foreign build systems support
We also provides an extension to the `FetchContent` API [HermeticFetchContent](https://github.com/tipi-build/hfc) to use FetchContent as a full fledged package manager.

* ➡ Add `include(HermeticFetchContent)` to CMakelists
* 📘 Learn more in the [reference documentation](https://tipi-build.github.io/hfc/)
* 🚀 [Get Started with one of the example](https://github.com/tipi-build/hfc/tree/main/example)

## How to use it ?
Exactly as the CMake FetchContent documentation requires, the build will just be cached and faster to restore.
`export CMAKE_TIPI_PROVIDER_ENABLE=ON`

## How does it work ?
The speedup comes from the fact that instead of using `add_subdirectory` by default as plain FetchContent does, it runs the build of the dependency in a separate CMake context with `cmake-re` git mirroring based caching and installs the build artifacts in a dependency-specific sysroot.

When a cache entry is found, as it does not rely on CMake adding the subdirectory as part of the build, it doesn't need to fetch the sources but simply the **install-tree** artifacts.

Fetching the sources in big project can be slow, but also having to provide the exact same environments for the build to happen to get the built artifacts can also be an issue, tipi's smart caching is based on an [ABI-hash](./0360-build-cache.md) computation, that behaves as if the project was built from sources but without the cost of building if it was already done by a peer [in the same team](./0370-shared-cache.md).

If compile flags changes in an incompatible way, the build will be performed fully from sources again.

tipi installs the FetchContent provider by defining: [`-DCMAKE_PROJECT_TOP_LEVEL_INCLUDES=tipi_provider.cmake`](./tipi_provider.cmake).

It leverages the standard CMake [DEPENDENCY_PROVIDER](https://cmake.org/cmake/help/latest/command/cmake_language.html#dependency-providers) feature dedicated to integrate with FetchContent and find_package.

## Other C++ Package Managers
We have been working on integrating our build cache into the other common C++ package managers ( CPM, vcpkg, Conan... ). Contact us for more informations. 

## Help & Support
🧚 Get [community support](https://github.com/tipi-build/cmake-tipi-provider/issues)
<br/>📖 [Read CMake FetchContent documentation](https://cmake.org/cmake/help/latest/module/FetchContent.html)