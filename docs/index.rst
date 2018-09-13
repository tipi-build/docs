nxxm documentation : C++ dependencies & upgrade manager
=======================================================
Easing C++ development, inciting code reuse and improving application end-users experience by simplifying software updates. 

Getting started
===============
`Download nxxm <https://nxxm.github.io/#download>`_  and clone the `Get Started repository <https://github.com/nxxm/get-started>`_ repository.

nxxm features 
==============
To discover all the marvellous features nxxm offers you can take a look on our https://nxxm.github.io/ website.

* nxxm makes it intuitive to build a C++ project
    - Requires absolutely no build recipes
    - Builds by conventions with ``nxxm .``
* nxxm is a dependency manager for C++ which fetches and compile any C++ project hosted on `GitHub.com <https://github.com/>`_ . 
* adds software upgrade support to your apps.
* WebAssembly Ready but not only.

nxxm key principles
====================

Relaxing & flowing C++ 
----------------------
* Code scanning & conventions over build configuration
* 0 setup just coding

.. _every-project-lib-label:

Every project is a library
---------------------------
In a software project there are 2 kinds of entrypoints : 
  - Developers entrypoints for code reuse
  - End-user entrypoints for application use

Therefore the nxxm tools always prepare out of any project, even if it consists of a single C++ header and implementation file : a library that might be reused and applications that might be shipped.
    
Don't pay for what you don't use
--------------------------------
This is a core C++ design philosophy, and sadly the world of packages manager obliges you to take more than you need.

By definition a package is a pack of alot of things, and a developer won't need all of them.

nxxm allows you to do a fine-granular selection of your dependencies and pulls in your final application, thanks to modern C++ compilers only the needed bits.

Opinionated but compromise-ready
--------------------------------
While with nxxm you won't need anymore to write build files, you can still customize the parts or all with ``CMakeLists.txt.tpl`` files as we drive CMake internally. 

We don't encourage it though. 😇

CMake is a really robust solution that we cherish, but in our opinion it is a too low-level tool in a modern demanding C++ context.


Contents:
=========

.. toctree::
   :maxdepth: 8
   :glob:

   *
