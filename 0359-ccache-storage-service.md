---
title: 📦 ccache remote storage
aliases: [ ]
---

While `cmake-re` is optimized for CMake-based builds, it can also operate as a compiler, linker and archiver driver — making it possible to integrate with `ccache` to provide an RE-API backend as remote shared cache.

Once installed `cmake-re` will provide the following tools as part of it's distribution folder.

- `tipi-compiler-driver`
- `tipi-linker-driver`
- `tipi-ar-driver`
- `tipi-ranlib-driver`

## RE-API instead of ccache `remote_storage`
`ccache` supports it's own remote storage backend, our remote caching for ccache doesn't use this abstraction and instead relies on the more complete [Bazel RE-API](https://github.com/bazelbuild/remote-apis).

Unlike `ccache` `remote_storage` our integration enables caching static archives, shared objects and executables, also leveraging advanced compiler identification and system fingerprinting to prevent cache poisoning issues. 

The approach allows to maximizes cache HIT rates, with the ability to retrieve the full build graph from cache, not only compilation but also caching expensive linking operations, while reducing the amount of cache poisoning issues by being much more precise on the way cache keys are calculated.

### Using Bazel RE-API as remote `ccache`
These tools can then be configured to wire a remote cache to `ccache` via the RE-API, leveraging the `CCACHE_PREFIX` setting.

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
# Caching is better disabled during configure (so long 
# TIPI_INTERCALATED_COMPILER_LAUNCHER is unset no caching happens), as
# caching system probing operations will only store non really reusable
# cache entries.
export TIPI_INTERCALATED_COMPILER_LAUNCHER=rewrapper

make
```

#### CMake
If you have a CMake codebase [we advice to use the `cmake-re` wrapper](/documentation/0000-getting-started-cmake) which can further maximize cache HITs through automatic build hermeticity and containerization as it intercepts at the build system level instead of individual invocation only (those settings also being made in a smart fashion).

However if for some reasons you are not allowed to modify the `cmake` invocations and are just interested in plugging remote caching into `ccache` you can achieve it on any CMake Codebase as such :
```bash
# Wire ccache and RE-API Remote Caching
export CCACHE_PREFIX=tipi-compiler-driver

export CMAKE_CXX_COMPILER_LAUNCHER=ccache
export CMAKE_CXX_LINKER_LAUNCHER=tipi-linker-driver

# Plain CMake requires RANLIB / AR overriding to be executable not shell commands
echo "tipi-ranlib-driver /usr/bin/ar \$@" > configured-tipi-ar-driver && chmod +x configured-tipi-ar-driver
echo "tipi-ranlib-driver /usr/bin/ranlib \$@" > configured-tipi-ranlib-driver && chmod +x configured-tipi-ranlib-driver

cmake -S . -B ./build/cmake -G Ninja -DCMAKE_AR=$PWD/configured-tipi-ar-driver -DCMAKE_RANLIB=$PWD/configured-tipi-ranlib-driver

# Build
# Caching is better disabled during configure (so long 
# TIPI_INTERCALATED_COMPILER_LAUNCHER is unset no caching happens), as
# caching system probing operations will only store non really reusable
# cache entries.
export TIPI_INTERCALATED_COMPILER_LAUNCHER=rewrapper

cmake --build ./build/cmake
```

> #### Analyzing remote cache HITs
> In order to check the level of caching achieved and debug / improve it (e.g. detecting generated code files) it is possible to set the RBE_invocation_id as an UUID:
> 
> ```bash
> export RBE_invocation_id=`uuidgen`
> 
> # Clear local cache
> ccache -C
> ```
> 
> Then during the build all actions will be grouped in an EngFlow Profile that can be downloaded from the RE-API Cluster:
> 
> - `https://<cluster-address>/api/profiling/v1/instances/default/invocations/${RBE_invocation_id}`
> 
> The tracing file can then be analyzed with tools like Chrome Tracing Tools or Perfetto and will list the type of events (i.e. `actionCacheLookup`, `downloadBlob`) to confirm caching is working.