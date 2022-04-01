---
title: Dependencies and project configuration
order: 3
---
## Intro and Examples

_Tipi_ enables comfortable consumption of dependencies from multiple source like _GitHub.com_ repositories or _CMake Hunter_ provided packages.

Dependencies are specified in a project specific `.tipi/deps` file which can look like this:

```json
{
  "gh-user/repo": {},
  "tipi/gh": { "@" : "v0.0.1" },
  "gh-user/another-repo@file://repo-sub/folder": {},
  "file://local-subfolder": {},  
  "platform": [ "Boost::+boost" ]
}
```

This dependency specification ( *aka* depspec ) file tells _tipi_ on which libraries your project depends.

Every *key* of the JSON object represent a _library in repository URI_. 

- `"gh-user/repo"`: repository at `https://github.com/gh-user/repo`
- `"tipi/gh"`: repository at `https://github.com/tipi/gh`
- `"gh-user/another-repo@file://repo-sub/folder"`: library residing in `./repo-sub/folder/` of the _GitHub.com_ repository at `https://github.com/gh-user/another-repo`
- `"file://local-subfolder"`: library residing in the same file tree at `./local-subfolder`
- `"platform"`: however specifies a dependency on a commonly consumed package that is shipped as a part of the _tipi_ currated "platform" package (that have been tested and integrated on all the supported toolchains).

Dependencies from remote repositories will be fetched by `tipi` at build time and compiled by convention and finally installed into `./build/<platform>/sysroot`.

In this specific case :

- for `gh-user/repo` the latest revision of the default branch (usually `master`) will be fetched on every build[^1]. 
- for `tipi/gh` release/tag `v0.0.1` will be fetched once and always used from cache
- for `gh-user/another-repo@file://repo-sub/folder` the specified sub-folder of latests revision of the default branch will be fetched and built
- for `file://local-subfolder` the locally residing library in `./local-subfolder` will be built and installed
- Boost headers distribution will be downloaded once and installed to the build using to _CMake Hunter_.

## depspec syntax

The `<project>/.tipi/deps` file contais a JSON object whose **keys** are either _configuration entries for the local project_ **or** _library in repository URI_ and the corresponding **value** contains the configuration required to consume those libraries as C++ dependencies.

The simple example below would have _tipi_ always pull and compile the latest revision of the default branch of https://github.com/cpp-pre/json : 

```json
{
  "cpp-pre/json" : {}
}
```

The key `"cpp-pre/json"` is a github repository path (URI fragment "after" the `github.com/` URI). It can be any of the following : 

- `gh-user/gh-repo` : Github.com repository path
- `file://local/path` URI to represent a local dependency within the current project
- `gh-user/gh-repo@file://libs/sublibrary` dependency to a _part_ of a remote repository


In addition to the basic _library in repository_ information following configuration attributes can be provided to customize how the dependency should be consumed.

```json
{
  "<lib-in-repo-uri>" : {
    "@" : "<branch/tag/name>",
    "s" : ["<src-disambiguation>", ...], 
    "x" : ["<excluded-directory>", ...],
    "u" : <use-cmakelists::Boolean>,
    "packages": ["<Package Config name>", ...],
    "targets": ["<target name>", ...], 
    
    "@:<target>" : "<branch/tag/name>", 
    "s:<target>" : ["<src-disambiguation>", ...],
    "x:<target>" : ["<exclude dir>", ...],
    "u:<target>" : <use-cmakelists::Boolean>, 
    
    "requires" : { ... }
  },

  "platform[:target-platform]" : ["<dep>::<component>", ...],
  "platform[:target-platform]" : [{ "packages" : [], "targets" : [], "find_mode": "" }, ...],

  "s" : ["<src-disambiguation>", ...], 
  "x" : ["<excluded-directory>", ...],
  "u" : <use-cmakelists::Boolean>,
  "s:<target>" : ["<src-disambiguation>", ...],
  "x:<target>" : ["<exclude dir>", ...],
  "u:<target>" : <use-cmakelists::Boolean>, 
  "packages": ["<Package Config name>", ...],
  "targets": ["<target name>", ...], 
  "find_mode": "",
  "requires" : { ... }
}
```

> All configuration attributes are optional and can be ommitted.

### Details

#### - `@` : tag or branch name

- if *ommited* the default branch of the dependency is fetched on every build (unless `-n` used [^1])
- if a **tag** is specified is fetched once, then will be pulled from local cache on subsequent build
- if a **branch name** is specified, the latestest revision of the _branch_ is fetched on every build (unless `-n` used [^1])

> You can suffix the key with the target platform to selectively use a specific dependency version on certain platforms, e.g.
> specifying `"@:wasm-cxx17" : "v0.0.1"` will select the version v0.0.1 for the `WebAssembly` target and fall back to the latest
> revision of the default branch for all other targets
>
> This attribut **cannot** be specified in the _local project context_

#### - `s` : source dir disambiguation

If a repository contains multiple sources directories with uncommon name they can be added to the list of includes 
or files to link with by using the source dir disambiguation attribute `s`.

*This disables most of the smart inclusion detection and lists the paths in the build process.*

> As for `@` selective inclusion by platform can be specified by adding a target platform suffix: 
>
> Adding `"s:vs-16-2019-win64-cxx17" : ["src/visual-c"]` will compile with the `src/visual-c` on vs-16-2016 target.

#### - `x` : exclude directory from scan

Directories can be explicitely excluded from source scanning by listing them in the `x` attribute. Directories starting with a `.` (dot) are ignored by default

> You can suffix the key it with the target platform to selectively include implementation dir by platform
>  
> Specifying ``"x:wasm-cxx17" : ["src/native-code"]`` will compile the project without the sources found in the `src/native-code` directory for the WebAssembly target.

#### - `u` : use CMakeLists

Setting this to `true` will disable the convention build and have tipi rely on the `CMakeLists.txt` file found in the project.

> Note: this property expect a `boolean` value, so the deps specification shall not contain quotation marks 

#### - `packages`, `targets`

Useful in combination with the option `u` / use CMakeLists as it allows to set the packages and targets we expect from the dependency to be searched for via CMake find_package.

The following example shows how to build the library `libgit2` using the provided project CMakeLists and it's own specific targets.

```json
{
  "tipi-build/libgit2" : { 
    "@" : "v1.1.0-cmake-findpackage", 
    "u" : true,
    "packages": ["libgit2"], "targets": ["libgit2::git2"] 
  }
}  
```

#### - `requires` sub-depspec

When using non-tipi dependencies the `requires` attribute allows you to specify additional dependencies.

```json
{
  "arun11299/cpp-jwt" : { 
    "@" : "master", 
    "x" : ["/tests/" ,"examples/", "include/jwt/test"],
    "requires" : { 
      "nlohmann/json" : { "@" : "v3.1.2" }
      <...things omitted...>
    }    
  }
}  
```

With the value of `requires` being a full "sub-depspec" (with unlimited nesting).

This attribution can be useful to change a transitive dependency, for example if you prefer to use `BoringSSL` in place of `OpenSSL` for a libary which would depend on `OpenSSL`.

#### - `platform[:target-platform]` tipi platform libraries

You can specify _tipi_ provided dependencies from a currated list of platform dependencies[^2]. These libraries are considered to be widely used and are generally tested on the supported platforms and environments.

```json
"platform[:target-platform]" : ["<dep>::<component>", ...]
```

By adding a `:target-platform` suffix dependencies can be selectively included only for certain targets.

If both `platform` and a matching `platform::target` the union set of both will be used.

The platform library dependencies have to be specified as follows:

- `"PackageName::+component"` if the component is an option of PackageName to be linked but is always shipped with PackageName ( *e.g.* header only Boost distribution via `"Boost::+boost"` if our project uses that)
- `"PackageName::component"` if the component is to be linked and needs to be fetched separately. ( *e.g.* `"Boost::filesystem"` is not shipped in the header only distribution of Boost, so it has to be declared explicitely) 
- `"target::native-name"` if the component is already installed on the environment and should be used. ( *e.g.* linking against `libdl.so` on Linux can be specified by adding `"linux::dl"` )

##### CMake built platform dependencies
Further narrowing the specification for CMake `find_package` by setting `packages`, `targets` and `find_mode` can be achieved by:

```json
"platform[:target-platform]" : [{ "packages" : [], "targets" : [], "find_mode" : "" }, ...]
```

This can be useful for platform packages that need to be imported in a specific way, for example when accomodating for the use of complex systems like PkgConfig or because the package needs to be searched in CMake `CONFIG` modes.

An example would be depending on the ICU unicode library : 

```json
"platform":[{"find_mode":"CONFIG", "packages" : ["ICU"], "targets" : ["ICU::uc"] }]
```

Note: For a list of available platform libraries please refer to [^2] .

### platform packages vs tipi dependencies

_tipi_ was built around a few opinionated choices among which was the descision to provide the ability to consume widely used C++ libraries via the "platform" library specification.

This makes their usage more common and via a single inclusion without needing to search the exact repository on github.

### Local project configuration

In order to provide configuration options for the local project (the project containing the `.tipi/deps` file) you can use the same configuration attributes as
for dependencies at the root level of the configuration object.

```json
{
    "s" : ["<src-disambiguation>", ...], 
    "x" : ["<excluded-directory>", ...],
    "u" : <use-cmakelists::Boolean>,
    "s:<target>" : ["<src-disambiguation>", ...],
    "x:<target>" : ["<exclude dir>", ...],
    "u:<target>" : <use-cmakelists::Boolean>, 
    "packages": ["<Package Config name>", ...],
    "targets": ["<target name>", ...], 
    "find_mode": "",
    "requires" : { ... }
}
```

> All configuration properties except the `@` section are available, including the option to specify build target specific source desambiguation or inclusion rules.

[^1]: Unless the `-n` switch is used which then uses any previously downloaded revision

[^2]: Tipi relies on the structure provided by the platform base project for platform dependencies. You can explore it here: https://github.com/nxxm/hunter/blob/develop/cmake/configs/default.cmake
