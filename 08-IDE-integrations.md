---
title: IDE Integration
order: 10 
---

# Integration in IDEs and Native build systems

_Tipi_ is available as a [command line tool](/documentation#getting-started) and an official [<img src="~/assets/vscode.png" style="height: 1em; vertical-align: middle;">&nbsp; Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=tipi.tipi-build)

Additionally it is easy to integrate _tipi_ with IDEs and other build systems as it relies on CMake internally. 

## Eclipse CDT Integration

By using a [CMake Generator](https://cmake.org/cmake/help/v3.18/manual/cmake-generators.7.html#cmake-generators) tipi can be nicely integrated 
with Eclipse CDT Integration, including custom targets to build using your tipi.build cloud subscription.

To setup an eclipse project with tipi, simply run: `tipi build . -G "Eclipse CDT4 - Unix Makefiles"` and open the project in Eclipse CDT.

<!--
Note @daminetreg: I don't get the feature. Please explain it to me asap

## Customizing default build_engine_mapping.json

When no Generators are provided, tipi either takes the best default or selects it from build_engine_mapping.json ( *c.f.* In TIPI_HOME_DIR : <distro-id>/environments/build_engine_mapping.json ).

This file allows to make the link between the target name vs-<XX>-<XXXX>-win64-cxx17 and the actual native build system used. It's mostly useful with windows MSVC which requires specific MSBuild version to be used.
-->
