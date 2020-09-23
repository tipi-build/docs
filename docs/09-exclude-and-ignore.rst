.. _exclude_ignore:

**************************
Exclude and Ignore
**************************


Exclude 
=================

If in your projects you compile your project but you want to exclude a directory. You can do this by using -x or --exclude. 
Followed by the name of the folder you want to exclude 
for example: --exclude=benchmark/ 

Moreover if you have several directories to exclude you can chain this option with several --exclude  

Ignore 
=================

The exclude method can be useful but becomes a bit problematic and repetitive if you always have to ignore certain directories in your projects.

To simplify this the nxxm team has developed the ignore option which is called with : -i or --ignore 

You can create an ignore file in three different places on your computer :
 
- You can create an ignore file in the distribution of nxxm. This file will be taken into account for all your builds.

.. hint:: On Windows it's in `C:\\\\.nxxm\\\\distro\\\\00000X\\\\ignore`
.. hint:: On other platforms in `${HOME}/.nxxm/distro/00000X/ignore`

- You can create an ignore file in the directory of nxxm. This file will be taken into account for all your builds.

.. hint:: On Windows it's in `C:\\\\.nxxm\\\\ignore`
.. hint:: On other platforms in `${HOME}/.nxxm/ignore`

- You can create an ignore file in your directory of your project. This file will be taken only for your project.
  To do this in your project create the directory .nxxm if it doesn't exist and then the file ignore.
  
  







