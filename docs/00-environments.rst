.. _tipi-build-environment-label:

***********************
tipi.build environments
***********************
A tipi.build environment consits of 2 elements : 
    - CMake toolchain files 
    - OS image

The OS images are described via the help of Packer files and can be automatically deployed on the tipi.build cloud as many-core build agent or as execution environment for unit test.

Officially supported environments are present as ``<environment>.pkr.js + <environment>.cmake `` file pairs in the `tipi-build/environments <https://github.com/tipi-build/environments>` github repository.

While tipi embeds a big set of toolchain files and also welcome community maintained or private environments it emphasizes on providing a default environment for each major platform.

Hence tipi.build always provides a default clang up-to-date with libc++ STL for :
    - Webassembly : ``tipi build . -t wasm``
    - Windows :  ``tipi build . -t windows``
    - Linux :  ``tipi build . -t linux``
    - macOS :  ``tipi build . -t macos``

Variations specifiying C++ standard versions are also available.

.. hint:: The ``.`` dot in the commands can be any path containing C++ source files or a C++ CMake project.

Building in the tipi.build cloud
================================

  tipi build . -t linux-cxx17 

This will launch the ``~/.tipi/<distro>/environments/windows.pkr.js`` environment on tipi.build and run the build on a massive parallel build farm.

A file synchronization mechanism will bring the data back to your machine if you either need to transmit them or run them.

Building on your local machine
==============================

  tipi . -t linux-cxx17

When your machine fits your target environment you can also use the tipi toolchain to build directly on your machine.

Users would typically build others target platform on tipi cloud, to check their code don't break or work on these platform.

Running apps on different or remote environments
==================================================

  tipi -t linux-cxx17 .run build/bin/app

This will use a remote environment of the target type to run the app.

Running within the tipi environment for your local machine
==========================================================

  tipi run build/bin/app

Will run a command from within the tipi environment based on your local machine. The absence or ``.`` in front of run makes it local.

Building your own environment
==============================
It is possible for tipi to build your own environment, the only step you need to follow is writing a CMake toolchain file and a Packer file.

Examples can be found at `tipi-build/environments <https://github.com/tipi-build/environments>` .

Once they are stored in ``~/.tipi/<distro>/environments/<environment>.pkr.js`` and ``~/.tipi/<distro>/environments/<environment>.cmake`` a ``tipi build .  -t <environment>`` will create it in the tipi cloud and build the current sources in it.