**************************
Integration in IDEs and Native build systems
**************************

tipi by default uses it's own build system and is provided as a command line client and an official `VSCode Plugin <https://marketplace.visualstudio.com/items?itemName=tipi.tipi-build>`_

Still it's possible to integrate tipi in different IDEs and build systems, as tipi relies on CMake internally.

Hence it's possible to provide a `CMake Generator <https://cmake.org/cmake/help/v3.18/manual/cmake-generators.7.html#cmake-generators>`_

Eclipse CDT Integration
=======================
tipi augments the Eclipse CDT Integration by properly setting the source directory and adding custom targets to build on the tipi.build remote compiler cloud.

To setup an eclipse project with tipi, simply run : ``tipi build . -G "Eclipse CDT4 - Unix Makefiles" ``


build_engine_mapping.json
=========================
When no Generators are provided, tipi either takes the best default or selects it from build_engine_mapping.json ( *c.f.* In TIPI_HOME_DIR : <distro-id>/environments/build_engine_mapping.json ).

This file allows to make the link between the target name vs-<XX>-<XXXX>-win64-cxx17 and the actual native build system used. It's mostly useful with windows MSVC which requires specific MSBuild version to be used.