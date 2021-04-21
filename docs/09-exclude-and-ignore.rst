**************************
Ignore
**************************

Ignore 
=================

You can create an ignore file calling .nxxmignore in your project.
Files can be ignored via .nxxmigore files which support the .gitignore syntax ( Official documentation of .gitignore  https://git-scm.com/docs/gitignore)

=================
Examples
=================

```
  # exclude everything except directory foo/bar
    /*
    !/foo
    /foo/*
    !/foo/bar
```

```
 # exclude every cpp with a number or swp in the name 
*.swp.cpp
[0-9].cpp
```





