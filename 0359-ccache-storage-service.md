---
title: 📦 L2 remote caching with `ccache`
aliases: [ ]
---

While `cmake-re` is optimized for CMake-based builds and automatically provides remote caching for CMake builds, it can also operate as a compiler, linker and archiver launcher — making it possible to combine RE-API remote cache with a local `ccache`.

Once installed `cmake-re` will provide the following tools as part of it's distribution folder.

- `tipi-compiler-driver`
- `tipi-linker-driver`
- `tipi-ar-driver`
- `tipi-ranlib-driver`
- `reproxy`
- `rewrapper`

## RE-API instead of ccache `remote_storage`
`ccache` supports its own remote storage backend, our solution to integrate remote caching for ccache doesn't use this abstraction and instead relies on the more complete [Bazel RE-API](https://github.com/bazelbuild/remote-apis), combining the best of both systems.

Unlike `ccache` `remote_storage` we extend caching beyond translation-unit compilation, which is the limit of native ccache. Our integration enables caching static archives, shared objects and executables, also leveraging advanced compiler identification and system fingerprinting to prevent cache poisoning issues. 

The approach allows to maximize cache HIT rates, with the ability to retrieve the full build graph from cache, not only compilation but also caching expensive linking operations, while reducing the amount of cache poisoning issues by using more precise cache entry matching.

### Using Bazel RE-API as remote shared `ccache` layer
Here is how to configure `ccache` to leverage remote caching on an EngFlow RE-API cluster by using the `CCACHE_PREFIX` setting.

1. Install necessary tools

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

2. Setup a virtual environment with `tipi run bash`

3. [Authenticate to the remote caching service](/documentation/0352-distributed-builds#authenticate-with-an-mtls-certificate) by setting `RBE_service`,`RBE_tls_client_auth_key`,`RBE_tls_client_auth_cert`

4. Configure remote caching, from within your project:
```bash
# Remote Caching, Local Build execution
export RBE_exec_strategy=local
export RBE_exec_root=$PWD
export RBE_platform="InputRootAbsolutePath=$PWD"

# Start remote execution proxy
touch $PWD.unix-sock-reproxy
export RBE_server_address=unix://$PWD.unix-sock-reproxy
export RBE_reproxy_wait_seconds=5
export RBE_service_no_auth="true"
export RBE_use_application_default_credentials="true"
reproxy & 
```

5. Configure the project to use ccache and advanced caching for linking
  - Autotools / Makefiles
  - CMake

#### Autotools / Makefiles
```bash
# Wire ccache and RE-API Remote Caching
export CCACHE_PREFIX=tipi-compiler-driver

export CXX="ccache c++"
export CC="ccache cc"
export LD="tipi-linker-driver /usr/bin/ld"
export AR="tipi-ranlib-driver /usr/bin/ar"
export RANLIB="tipi-ranlib-driver /usr/bin/ranlib"

# Configuration
./configure

# Build
# Caching is better disabled during configuration steps.
# When the environment vairable TIPI_INTERCALATED_COMPILER_LAUNCHER is not set,
# no calls to the RE-APIs are made and all work is local. 
# The compiler invocations for configuration purposes are faster to run
# locally as they are usually using temporary files that can't be cached.
export TIPI_INTERCALATED_COMPILER_LAUNCHER=rewrapper

make
```

#### CMake
If you have a CMake codebase [we advise to use `cmake-re`](/documentation/0000-getting-started-cmake) which can further maximize cache HITs through automatic build hermeticity and containerization as it manages caching at the build system level instead of individual invocation only.

It is also possible to integrate manually with cmake and `ccache` to benefit from local caching and our RE-API based remote caching. Here is an example of such an integration:

```bash
# Wire ccache and RE-API Remote Caching
export CCACHE_PREFIX=tipi-compiler-driver

export CMAKE_C_COMPILER_LAUNCHER=ccache
export CMAKE_CXX_COMPILER_LAUNCHER=ccache
export CMAKE_CXX_LINKER_LAUNCHER=tipi-linker-driver

# Plain CMake requires RANLIB / AR overriding to be executable not shell commands
echo "tipi-ranlib-driver /usr/bin/ar \$@" > configured-tipi-ar-driver && chmod +x configured-tipi-ar-driver
echo "tipi-ranlib-driver /usr/bin/ranlib \$@" > configured-tipi-ranlib-driver && chmod +x configured-tipi-ranlib-driver

cmake -S . -B ./build/cmake -G Ninja -DCMAKE_AR=$PWD/configured-tipi-ar-driver -DCMAKE_RANLIB=$PWD/configured-tipi-ranlib-driver

# Build

export TIPI_INTERCALATED_COMPILER_LAUNCHER=rewrapper

cmake --build ./build/cmake
```

> #### Analyzing remote cache HITs
> In order to download build caching statistics for later analysis (improving cache hit rate or detecting always changing files), you can set the environment variable `RBE_invocation_id` as an UUID. Each separate builds should have a unique value.
> 
> ```bash
> export RBE_invocation_id=`uuidgen`
> 
> # Clear local cache
> ccache -C
> ```
> 
> All actions will be grouped in an EngFlow Profile using that UUID. The profile can be downloaded from the RE-API Cluster after the build completed:
> 
> - `https://<cluster-address>/api/profiling/v1/instances/default/invocations/${RBE_invocation_id}`
> 
> The tracing file can then be analyzed with tools like Chrome Tracing Tools or Perfetto and will list the type of events (i.e. `actionCacheLookup`, `downloadBlob`) to confirm caching is working.