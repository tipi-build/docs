
.. _dependencies-spec:

**************************
Dependencies Specification
**************************

If your project isn't dependency free then you can consume any GitHub.com repository or CMake Hunter Provided packages.

This simply can be specified in a ``.nxxm/deps`` which can look like this::

  {
      "gh-user/repo" : {}

    , "nxxm/gh"      : { "@" : "v0.0.1" }

    , "platform" : [ "Boost::+boost" ]
  }


This dependency specification ( *i.e.* depspec ) file tells which libraries your project needs.

Every *key* of the JSON object represent a github URI. 

- "gh-user/repo": repository at https://github.com/gh-user/repo
- "nxxm/gh": repository at https://github.com/nxxm/gh
- "platform" however specifies a commonly consumed package that have been tested and integrated on all the toolchains supported by nxxm.

The effect of having such a ``.nxxm/deps`` file is that on ``nxxm .`` call the GitHub.com repositories will be fetched, compiled by convention and installed within the ``/build/<platform>/sysroot``.

In this specific case :

* "gh-user/repo" default branch ( usually master ) will be taken and always the latest. Unless ``nxxm . -n`` is passed which relies then only on it's previously downloaded cache.
* nxxm/gh tag v0.0.1 will be fetched once and always used
* Boost headers distribution will be downloaded once and installed to the build thanks to CMake Hunter.


.nxxm/deps syntax
=================
A JSON object whose **keys** are GitHub URI and values configurations to consume those repositories as C++ libraries dependencies.

Simple example that would always pull and compile the latest state of the default branch of https://github.com/cpp-pre/json : 

::

  {
     "cpp-pre/json" : {}
  }

gh-user/gh-repo
---------------
The following attributes are possible to declare how the dependencies should be consumed.

::

  {
      "gh-user/repo" : {
          "@" : "<branch/tag/name>"
        , "s" : ["<src-disambiguation>", ...]
        , "x" : ["<exclude dir>", ...]
        , "u" : <use-cmakelists>
        , "packages": ["<Package Config name>", ...]
        , "targets": ["<target name>", ...]

        , "@:<target>" : "<branch/tag/name>"
        , "s:<target>" : ["<src-disambiguation>", ...]
        , "x:<target>" : ["<exclude dir>", ...]
        , "u:<target>" : <use-cmakelists>
        
        , "requires" : { ... }
      }

     "platform[:target-platform]" : ["<dep>::<component>", ...]
     "platform[:target-platform]" : [{ "packages" : [], "targets" : [] }, ...]

  }

.. tip:: The attributes are optional. They can all be ommitted.

@ : tag or branch name
^^^^^^^^^^^^^^^^^^^^^^
- *ommited* : in this case the default branch is taken is fetched each time for the latest version unless ``-n`` is passed to nxxm.
- a **tag** is fetched only once, then the version is kept. 
- a **branch name** is fetched each time for the latest version unless ``-n`` is passed to nxxm.
- You can suffix the key it with the target platform to selectively use a dependency in different platforms.
  
  e.g. ``"@:wasm-cxx17" : "v0.0.1"`` will select the version v0.0.1 for WebAssembly but not for the native platforms

s : source dir disambiguation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
If a repository has alot of sources directories with uncommon name they can be added to the list of includes or files to link with s. 

You can suffix the key it with the target platform to selectively include implementation dir by platform
  
  e.g. ``"s:vs-15-2017-win64-cxx17" : ["src/visual-c"]`` will compile with the ``src/visual-c`` on vs-15-2017 but not on other targets.

x : directory to completely ignore
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Directories that are unneeded to scan. Usually you don't need to specify this.
Note that directories starting with a .dot will always be ignored.

You can suffix the key it with the target platform to selectively include implementation dir by platform
  
  e.g. ``"x:wasm-cxx17" : ["src/native-code"]`` will compile without the native code directory for the WebAssembly platform.

u : use CMakeLists
^^^^^^^^^^^^^^^^^^
Per default nxxm scans the source code of your application and dependencies to build it automagically.
If that is not wanted it is possible to specify ``"u" : true`` to use the CMakeLists.txt of the project.

packages,  targets
^^^^^^^^^^^^^^^^^^
Useful in combination with the option to use CMakeLists from dependency ( i.e. ``"u" : true`` ), it allows to set the packages and targets we expect from the dependency to be searched for via CMake find_package.

Here follows an example to build the library libgit2 with it's own CMakeLists and it's own specific targets.

::

  {
    "nxxm/libgit2" : { 
      "@" : "v1.1.0-cmake-findpackage", 
      "u" : true,
      "packages": ["libgit2"], "targets": ["libgit2::git2"] 
    }
  }  

requires
^^^^^^^^
The requires is a way to adapt a non nxxm dependency which also has dependencies, there are no limits on the nesting you can use. 

It is also really useful to change a transitive dependency, for example if you prefer to use BoringSSL in place of OpenSSL for a libary which would depend on OpenSSL.



platform[:target-platform]
^^^^^^^^^^^^^^^^^^^^^^^^^^
.. tip:: For a list of possible platform libraries please refer to :ref:`package-list`.

::

  "platform[:target-platform]" : ["<dep>::<component>", ...]

It's possible to specify dependencies that we consider platform provided. Meaning they are really common and used accross almost any project, but still needs to be specified.

``:target-platform`` can be appended to selectively include dependencies only on certain target platform, hence the key name. The target platform is selected after the `nxxm -t target-platform` parameter.

If there is a ``platform`` and a ``platform::target`` both will be used together. 

The platform libraries have to be specified as follow :

- "PackageName::+component" if the component is an option of PackageName to be linked but is always shipped with PackageName ( *e.g.* header only Boost distribution via "Boost::+boost" is always shipped, we need to declare that we use it.).

- "PackageName::component" if the component is to be linked and needs to be fetched separately. ( *e.g.* "Boost::filesystem" is not shipped per-se by Boost it must be declared as to install in sysroot first." ).
 
- "target::native-name" if the component is already installed on such platforms and should be used. ( *e.g.* linkign to libdl.so on linux can be specified by ``target::dl`` )

::

  "platform[:target-platform]" : [{ "packages" : [], "targets" : [] }, ...]
  
Allows to set the packages and targets we expect from the dependency to be searched for via CMake find_package.

This can be useful for platform packages that need to be imported in a specific way, for example accomodating use of complex systems like PkgConfig.

.. tip:: For a list of possible platform libraries please refer to :ref:`package-list`.

platform vs GitHub.com
""""""""""""""""""""""
We made the choice to provide the ability to consume well-known C++ libraries via the "platform" library specification.

This makes their usage more common and via a single inclusion without needing to search the exact repository on github.


