tipi documentation : C++ compiler as a Service
==============================================
Easing C++ development, inciting code reuse and speeding up build workflows !

Getting started
===============
`Download tipi <https://tipi.build/>`_  by signing in and follow the online setup wizard.

tipi features 
==============
To discover all the marvellous features tipi offers you can take a look on our https://tipi.build/ website.

* tipi speeds up your workflow with autoprovisioned cloud environments : toolchain, compilers, tools.
* tipi is a dependency manager for C++ which fetches and compile any C++ project. 
* tipi builds superfast with cloud powered build distribution

tipi key principles
====================

Relaxing & flowing C++ 
----------------------
* Code scanning & conventions over build configuration
* 0 setup just coding

  - Just select one environment from our `Supported list <https://github.com/tipi/environments/tree/master/>`_ or specify your own.
  - tipi.build will download & install the compiler and libraries automatically in an isolated distro folder.

.. _every-project-lib-label:

Every project is a library
---------------------------
In a software project there are 2 kinds of entrypoints : 
  - Developers entrypoints for code reuse
  - End-user entrypoints for application use

Therefore the tipi tools always prepare out of any project, even if it consists of a single C++ header and implementation file : a library that might be reused and applications that might be shipped.
    
Don't pay for what you don't use
--------------------------------
This is a core C++ design philosophy, and sadly the world of packages manager obliges you to take more than you need.

By definition a package is a pack of alot of things, and the final application won't need all of them.

tipi allows you to do a fine-granular selection of your dependencies and pulls in your final application, thanks to modern C++ compilers only the needed bits.

Opinionated but compromise-ready
--------------------------------
While with tipi we push for builds without complex scripts, you can still customize the parts or all with ``CMakeLists.txt.tpl`` or ``CMakeLists.txt`` associated with ``use-cmake.tipi`` files as tipi is built on top of CMake, which has a great acceptance in the C and C++ community.

In our opinion CMakeLists should be used only were real is a really robust solution that we cherish, but in our opinion it is a too low-level tool for a developer to face in daily development tasks. 


Contents:
=========

.. toctree::
   :maxdepth: 8
   :glob:

   *
