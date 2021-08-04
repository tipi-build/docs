**************************
Ignore
**************************

Ignore 
=================

You can create an ignore file named ``.tipiignore`` in your project.
Files can be ignored via **.tipiignore** files whose syntax is **.gitignore** ( Official documentation for .gitignore  https://git-scm.com/docs/gitignore)

:tip: Files that are ignored by your _.gitignore_ file are also ignored during tipi code scan, however you can avoid this by adding the same rule with a ``!`` symbol in front in your _.tipiignore_ .

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


