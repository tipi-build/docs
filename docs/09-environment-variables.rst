*********************************************
Environment variables
*********************************************

NXXM_HOME_DIR (dependencies & tools cache)
==========================================
When using nxxm for the first time, the software will create a new directory `.nxxm`

  - On Windows it's in `C:\\\\.nxxm\\\\`
  - On other platforms in `${HOME}/.nxxm/`

In this directory nxxm will install dependencies, toolchain files and tools for your environments.

But you may not have permission to write to this part of the disk. Or that you run nxxm from another disk.

To solve this problem you can tell nxxm to install it's tools and dependencies at the location pointed by : `NXXM_HOME_DIR`.

For example on windows powershell you can specify `$env:NXXM_HOME_DIR = "D:\\\\a\\\\.nxxm"`



NXXM_DISTRO_JSON
================
nxxm uses a json file which contains the required tools used by nxxm to build projects, as cmake or make. These tools are automatically downloaded and installed by nxxm at runtime before running projects build.

To allow nxxm usage flexibility, nxxm is ready to use the environment variable `NXXM_DISTRO_JSON` which may point to an absolute or relative file path, or even an HTTP(s) url which point to your own distribution json file.

The original json file can be found at https://nxxm.github.io/distro/distro.json.

Below some examples of what you can set as `NXXM_DISTRO_JSON`:

  - NXXM_DISTRO_JSON = "~/projects/nxxm/distro.json"
  - NXXM_DISTRO_JSON = "/home/user/projects/nxxm/distro.json"
  - NXXM_DISTRO_JSON = "https://company.com/nxxm/distro.json"

If `NXXM_DISTRO_JSON` contains an absolute or relative file path, it will be used directly by nxxm.

If `NXXM_DISTRO_JSON` contains an HTTP(s) url, nxxm will handle the file download and the environment variable `NXXM_DISTRO_JSON_SHA1` has to be defined too (see :ref:`NXXM_DISTRO_JSON_SHA1`).


NXXM_DISTRO_JSON_SHA1
=====================

As nxxm handles the environment variable `NXXM_DISTRO_JSON`, if it contains an HTTP(s) url, nxxm will check the SHA1 got from the environment variable `NXXM_DISTRO_JSON_SHA1` to know if it has to download and override the existing json file.

For example:
  - NXXM_DISTRO_JSON = "https://company/nxxm/distro.json"
  - NXXM_DISTRO_JSON_SHA1 = "4eb777d088ea949709e9ea97bbc8c389a63856e2"
