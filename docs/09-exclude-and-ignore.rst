.. _exclude_ignore:

**************************
Exclude and Ignore
**************************


Exclude 
=================

If in your projects you compile your project but you want to exclude a directory. You can do this by using -x or --exclude or -i or --ignore . 
Followed by the name of the folder you want to exclude 
for example: --exclude=benchmark/ 

Moreover if you have several directories to exclude you can chain this option with several --exclude  

Ignore 
=================

The exclude method can be useful but becomes a bit problematic and repetitive if you always have to ignore certain directories in your projects.

To simplify this the nxxm team has developed the ignore option 

You can create an ignore file calling .nxxmignore in your project.
This file must respect the rules of gitignore.
These new rules will allow you to always ignore files or directories when compiling your software.







