*********************************************
Dependencies of nxxm
*********************************************

When using nxxm for the first time, the software will create a new directory `.nxxm` 
  - On Windows it's in `C:\\\\.nxxm\\\\`
  - On other platforms in `${HOME}/.nxxm/`

In this directory nxxm will install these dependencies.

But you may not have permission to write to this part of the disk. Or that you run nxxm from another disk.
For example on windows if nxxm is on D: it won't be able to install these dependencies correctly on C:

To solve this problem you can tell nxxm or install this `.nxxm` folder.
To do this you need to modify an environment variable : `NXXM_HOME_DIR` by giving this variable the path to the directory where you want nxxm to install these dependencies. 

For example on windows you can make  ` $env:NXXM_HOME_DIR = "D:\a\.nxxm"

