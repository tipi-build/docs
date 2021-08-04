**************************
`.tipiignore` Exclud and Ignore
**************************

tipiignore files
=================

You can create an ignore file named ``.tipiignore`` in your project.
Files can be ignored via **.tipiignore** files whose syntax is **.gitignore** ( Official documentation for .gitignore  https://git-scm.com/docs/gitignore)

.. hint:: Files that are ignored by your _.gitignore_ file are also ignored during tipi scan, however you can counter this by adding the same rule with a ``!`` symbol in front in your _.tipiignore_ .

.. hint:: Ignore rules can be provided as well via the `-x <ignorerule>` command line switch

=================
Examples
=================

::

  # exclude everything except directory foo/bar
  /*
  !/foo
  /foo/*
  !/foo/bar

::

  # exclude every .cpp ending by a number or by .swp.
  [0-9].cpp
  *.swp.cpp


