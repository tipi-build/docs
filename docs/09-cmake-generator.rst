**************************
Cmake generator and tipi
**************************

General rule
=================

Basically if you build on a Unix-like machine, tipi chooses to generate Unix Makefiles.
If you build on a Windows machine with clang (target windows for tipi), tipi chooses to generate MinGW Makefiles.
If you build on a Windows machine with msvc, tipi has a file named build_engine_mapping.json (found at .tipi/<distroId>/environment/build_engine_mapping.json,
This file allows to make the link between the target vs-<numero-of-vs>-<year-of-vs>-win64-cxx17 and the generator which must be used.

=================
Case of particular generator
=================

You can override these basic tipi rules by providing the -G option which allows you to give the generator of your choice. 
For example :  -G "Eclipse CDT4 - Unix Makefiles"
see the cmake documentation for generators available at https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html 
Moreover if you want to use another generator it must be present on your PATH.
Tipi has to test only "Eclipse CDT4 - Unix Makefiles" and "Eclipse CDT4 - MinGW Makefiles" 

