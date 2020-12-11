*********************************************
nxxm main directory (dependencies & tools cache)
*********************************************

When using nxxm for the first time, the software will create a new directory `.nxxm` 

  - On Windows it's in `C:\\\\.nxxm\\\\`
  - On other platforms in `${HOME}/.nxxm/`

In this directory nxxm will install dependencies, toolchain files and tools for your environments.

But you may not have permission to write to this part of the disk. Or that you run nxxm from another disk.

To solve this problem you can tell nxxm to install it's tools and dependencies at the location pointed by : `NXXM_HOME_DIR`.

For example on windows powershell you can specify `$env:NXXM_HOME_DIR = "D:\a\.nxxm"`
