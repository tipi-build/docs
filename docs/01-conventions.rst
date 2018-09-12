*********************
Builds by Conventions
*********************


Why ?
=====
Why did we ever write makefiles ? CMakeLists.txt or configured IDE project settings ? And why can we stop ?

Andrew Koenig once coined FTSE, the `Fundamental theorem of software engineering <https://en.wikipedia.org/wiki/Fundamental_theorem_of_software_engineering>`_ : 

  "We can solve any problem by introducing an extra level of indirection."

Actually building application is an extremely complex problems, and the layers are almost infinite : linker, assembler, compiler, frontend, build-system ( *e.g.* make), meta-build-system ( *e.g.* cmake) to quote only a few.

nxxm leverages all the fabulous work done in these layer to finally make building C++ a simple human task.

Because
--------

* C++ code expresses enough what is an application entrypoint and what is reusable library code.
* No need to learn a new language to specify how to build.
* We are tired of specifying each single file that should be built.


Convention by example
=====================
Take the idea of building a game project. This game project will contain : 

1. the game domain with the player characters, game menu and so on. 
2. the game app itself
3. tools apps like game maps and texture editor.

The directory could look like this : 

.. image:: build-by-convention-00.png
   :alt: game directory tree as in nxxm demo

game.cpp has a main function each, therefore they are **apps** by convention.

The main function may look like : 

.. image:: build-by-convention-01.png
   :alt: game main function

The *src/* folder is scanned by nxxm which notices that there are *.hpp* and *.cpp* that don't have any entrypoint, therefore it defaults to the library code ( *i.e.* the game domain ).

Finally *tools/* isn't quite a good name for library code, and there are ``map_editor.cpp`` and ``texture_editor.cpp`` that both have ``main()`` functions.
Which makes them app entrypoint. Therefore the **apps** convention kicks in.

This **apps** convention allows to have supporting file aside, therefore common.cpp is linked to map_editor and texture_editor. The header common.hpp is accessible via ``#include "common.hpp"`` while the game_classes is exported on the compiler include dirs which makes them usable via ``#include <game_classes/*.hpp>``.

nxxm will give the following summary: 

.. image:: build-by-convention-02.png
   :alt: game main function

Resulting in a bin folder like the following:: 

  bin
  ├── game.exe                // game.cpp
  ├── demo.lib                // game_classes *.cpp
  └── tools
      ├── map_editor.exe      // common.cpp, map_editor.cpp
      └── texture_editor.exe  // common.cpp, texture_editor.cpp

Conventions Types
=================
First main convention : every project is a library. ( *c.f.* :ref:`every-project-lib-label` ). 

Out of each git repositories passed to the ``nxxm`` program one library is produced.

.. HINT:: All the conventions can be mixed and don't need to be declared beforehand, they are getting detected.

.. HINT:: While often unneeded due to the conventions mentioned below it is possible to specify or reduce the scope of a library within a project with ``nxxm -s library-dir`` or ``{ "s" : "library-dir" }``.

splitted libs
-------------
::

  .
  ├── include
  │   └── header.hpp
  └── src
      └── impl.cpp

One typical kind of c++ project is when headers and implementation files are splitted in different folders, if it happens to be so nxxm will consider the include/ folder to be the publicly installable headers and src the implementation files constituing the library.

.. HINT:: By default nxxm checks the presence of ``include/ inc/ src/ sources/`` to infer this convention.

samedir libs
------------

::

  .
  └── src
      ├── header.hpp
      └── impl.cpp

Another typical convention C++ programmer uses are the implementation and headers together at the same level of directory hierarchy.

.. HINT:: By default nxxm checks the presence of ``src/ sources/`` to infer this convention and the absence of main() functions in the files.

toplevel libs
-------------
These are libraries that don't have any special source folder, their headers are directly rooted at the top of their repositories.

When this is detected the same mechanism as for **samedir libs** applies. 

.. HINT:: With this kind of structure it might be required to disambiguate nxxm to tell him which directories are really part of the lib thanks to ``nxxm -s library-dir`` or ``{ "s" : "library-dir" }``.

headeronly libs
---------------
It is possible to have code which is completely header only while application entrypoints are in .cpp files aside materializing either lib **examples** or a corresponding **app**.

The headers will be put at disposal like in the aforementioned conventions with ``#include <>``.


apps
----
any .cpp file with an entrypoint is an app.

For example any file containing a main() function or a macro instantiating a main() function ( *e.g.* unit testing frameworks ) will be compiled as an application. 

.. HINT:: apps are always linked to the project library.
.. HINT:: if others .cpp files are aside in the same or deeper filesystem directory they get linked with the applications in question. Except when those directories are part of the explicitely declared "s"/-s project library dir.

test or examples
----------------
Same as **apps** convention, however the project will register them within the CMake CTest test driver and calling ``nxxm . --test all`` will run them all and report result status


This convention kicks-in when files with main() functions in parent folder are named after : 

* test
* tests
* example
* examples

html
----
Any .html containing ``<script type="text/c++"></script>`` in it is compiled as an **app** convention.


Conventions are not enough
==========================
It could happen, please contact us so that we can improve nxxm or help you.

You can also tweak the build as explained below, this is however not recommended and goes against our vision. But we don't bite. :)

There is for sure a way for the convention build to work : less is more. Or put differently less CMakeLists is more time for your C++. (^^)

Override for one directory convention build
-------------------------------------------
This can be useful for really custom test framework or cases, you can give the hand to your CMake skills by adding in the subdiretories you don't want nxxm to do conventional builds.

Simply add an empty marker file `use-cmake.nxxm` and a valid `CMakeLists.txt`. The build will use CMake for this subpart.

Tweak nxxm convention build
---------------------------
We rely on CMake on you can tweak how we interract with it.

We don't recommend it but you can tweak fully or partially the build by adding ``CMakeLists.txt.tpl`` files in the main or sub directories of your project. 

To generate a sample CMakeLists.txt.tpl with the docs embedded of the different variables at your disposal call `nxxm cmaketpl`.

