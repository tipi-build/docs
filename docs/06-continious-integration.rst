*********************************************
Continous integration with GitHub actions 
*********************************************

With ``tipi ci`` you can generate a workflow that will build and test your project in github actions.

Create a Github secret
======================

In order for tipi to be able to pull from your private Github repositories, it needs the credentials you created for authentication_.

After creating it you can provide the TIPI_ACCESS_TOKEN, TIPI_REFRESH_TOKEN and TIPI_VAULT_PASSPHRASE as github secrets. See `tipi authentication variables`_ for more details.

.. _github_secrets_link: https://docs.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets

__ github_secrets_link_

.. _authentication: 05-authentication.rst

.. image:: tipi-ci.png
   :alt: Add tipi auth in github secrets

