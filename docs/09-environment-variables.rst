*********************************************
Environment variables
*********************************************

TIPI_HOME_DIR (dependencies & tools cache)
==========================================
When using tipi for the first time, the software will create a new directory `.tipi`

  - On Windows it's in `C:\\\\.tipi\\\\`
  - On other platforms in `${HOME}/.tipi/`

In this directory tipi will install dependencies, toolchain files and tools for your environments.

But you may not have permission to write to this part of the disk. Or that you run tipi from another disk.

To solve this problem you can tell tipi to install it's tools and dependencies at the location pointed by : `TIPI_HOME_DIR`.

For example on windows powershell you can specify `$env:TIPI_HOME_DIR = "D:\\\\a\\\\.tipi"`


TIPI_ENDPOINT (tipi.build instance)
===================================
tipi.build can be run on premise, this specifies the actual https API endpoint that tipi should use.


TIPI_ACCESS_TOKEN, TIPI_REFRESH_TOKEN (command line authentication)
===================================================================
These are the token that can be provided so that Continuous Integration machines can log into tipi and get access to the full tipi subscription.

See also `TIPI_VAULT_PASSPHRASE`

TIPI_VAULT_PASSPHRASE
=====================
When running login over environment variables, it is also required to decrypt the secure vault that tipi stores. This will be used to decrypt the vault.


TIPI_DISTRO_JSON
================
tipi uses a json file which contains the required tools used by tipi to build projects, as cmake or make. These tools are automatically downloaded and installed by tipi at runtime before running projects build.

To allow tipi usage flexibility, tipi is ready to use the environment variable `TIPI_DISTRO_JSON` which may point to an absolute or relative file path, or even an HTTP(s) url which point to your own distribution json file.

The original json file can be found at https://github.com/tipi/distro/blob/master/distro.json.

Below some examples of what you can set as `TIPI_DISTRO_JSON`:

  - TIPI_DISTRO_JSON = "~/projects/tipi/distro.json"
  - TIPI_DISTRO_JSON = "/home/user/projects/tipi/distro.json"
  - TIPI_DISTRO_JSON = "https://company.com/tipi/distro.json"

If `TIPI_DISTRO_JSON` contains an absolute or relative file path, it will be used directly by tipi.

If `TIPI_DISTRO_JSON` contains an HTTP(s) url, tipi will handle the file download and the environment variable `TIPI_DISTRO_JSON_SHA1` has to be defined too (see :ref:`TIPI_DISTRO_JSON_SHA1`).


TIPI_DISTRO_JSON_SHA1
=====================

As tipi handles the environment variable `TIPI_DISTRO_JSON`, if it contains an HTTP(s) url, tipi will check the SHA1 got from the environment variable `TIPI_DISTRO_JSON_SHA1` to know if it needs to download and override the existing json file.

For example:
  - TIPI_DISTRO_JSON = "https://company/tipi/distro.json"
  - TIPI_DISTRO_JSON_SHA1 = "4eb777d088ea949709e9ea97bbc8c389a63856e2"
