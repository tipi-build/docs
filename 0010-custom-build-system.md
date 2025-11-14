---
title: 🛸 Custom build systems
aliases: [ ]
---

While `cmake-re` is optimized for CMake-based builds, it can also operate as a compiler, linker and archiver driver — making it compatible with other build systems that are not directly supported (autotools, Make, Ninja, MSBuild) or as [distcc, ccache, sccache alternative](documentation/0359-ccache-storage-service).

In these cases, it can delivers most of the performance benefits of [⚡️ L2 Distributed Builds & Caching](/documentation/0352-distributed-builds), here follows how to use it.

The binary of `cmake-re` is distributed with the following tools :

- `tipi-compiler-driver`
- `tipi-linker-driver`
- `tipi-ar-driver`
- `tipi-ranlib-driver`

## Ecosystems and tools supported automatically
- `C` & `C++` : gcc,clang,msvc,ar,ranlib,ld,gold,lld,mold...
- `Java` : javac
- `TypeScript` : tsc

### `env:RBE_labels="type=tool"` : custom tools support
Through the specification of environment's variables `env:RBE_labels="type=tool"`, `env:RBE_input_list_paths` and `env:RBE_output_list_paths`, any custom tools can be supported and the generic `rewrapper` can be taught which file to cache and upload/download files necessary for the tool execution in a remote execution context.

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


2. Setup a virtual environment with `tipi run bash` / `tipi.exe run cmd`
3. [Authenticate to the remote caching service](/documentation/0352-distributed-builds#authenticate-with-an-mtls-certificate) by setting `RBE_service`,`RBE_tls_client_auth_key`,`RBE_tls_client_auth_cert`

4. Start remote execution proxy
```bash
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

5. Override compiler,linker and archiver command invocations.
#### Autotools / Makefiles
```bash
export CXX="tipi-compiler-driver c++"
export CC="tipi-compiler-driver cc"
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

#### Visual Studio msbuild 
If you have a CMake codebase [we advice to use the `cmake-re` wrapper](/documentation/0000-getting-started-cmake) but if you use self-maintained Visual Studio Solution, an integration could be done in the following way : 

Edit `<project>.vcxproj`, and **append at the end** after the line importing `Microsoft.Cpp.targets` : 
```xml
<PropertyGroup>
  <CLToolExe>tipi-compiler-driver.exe</CLToolExe>
  <CLToolPath>c:\.tipi\tipi-compiler-driver\<cmake-re-release-hash>\</CLToolPath>
</PropertyGroup>
```