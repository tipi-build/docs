
GitHub.com Dependencies
=======================
.. hint:: Specify any GitHub.com dependency as :ref:`dependencies-spec` 

.. to generate the list: 
.. ls -1 | while read line; do echo $line; fgrep target_link_libraries $line; done

.. _package-list:

Platform Dependencies Shorthands
================================
.. hint:: Specify the platform spec as in :ref:`dependencies-spec` 

We rely on the structure provided by the platform base project for the platform dependencies, you can see the version of the platform libraries used by default by ``tipi`` `in it's config <https://github.com/nxxm/hunter/blob/develop/cmake/configs/default.cmake>`_.
