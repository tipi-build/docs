*************************
Authentication
**************

tipi.build authentication allows accessing your tipi.build cores and nodes, it also allows tipi.build to access the source code repositories you need on Github.

It's possible to add additional authentication methods, user/password and tokens in your personal vault.

The vault is encrypted with strong cryptography on the user local machine. ( _c.f._ `Learn about the vault strong security <https://github.com/tipi-build/vault>` )

Authenticating to tipi.build
============================

Run the `tipi connect` command :

::

  tipi connect

This will prompt you with a link to authenticate the device on tipi.build, after downloading the vault the vault passphrase will be asked for.

.. note::
  You can connect to your own tipi.build instance by specifying the `TIPI_ENDPOINT` environment variable. 

.. hint:: The vault will be stored in `$TIPI_HOME_DIR/.tipi/.auth` .