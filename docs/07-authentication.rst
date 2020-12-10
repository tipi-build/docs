*********************************************
GitHub.com & Github Enterprise Authentication 
*********************************************

Create a .nxxm/.auth file
=========================

.. hint:: On Windows it's in `C:\\\\.nxxm\\\\.auth`
.. hint:: On other platforms in `${HOME}/.nxxm/.auth`

If you have modified the environment variable ($env:NXXM_HOME_DIR), the authentication token must be in the `$NXXM_HOME_DIR/.nxxm/.auth` .

The file is a JSON Array of credentials to setup on one entry GitHub.com credentials and on mutliple for Github Enterprise :

::

  [
    {
      "auth_info" : {
        "user" : "<GitHub.com username>",
        "pass" : "<GitHub.com password or Personal Access Token (if you have 2FA on)>"
      }
    },
    {
      "endpoint" : "https://github.your-enterprise.com",

      "auth_info" : {
        "user" : "<username>",
        "pass" : "<GitHub.com password or Personal Access Token (if you have 2FA on)>"
      }
    }

  ]

To create a personal access token, please refer to the `GitHub documentation <https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/> `_ .
