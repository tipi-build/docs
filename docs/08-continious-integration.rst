*********************************************
Continous integration with GitHub actions 
*********************************************

With ``tipi ci`` you can generate a workflow that will build and test your project in github actions.

Create a Github secret
======================

In order for tipi to be able to pull from your private Github repositories, it needs the credentials you created for authentication_.

After creating it you can copy its content and paste it in a `Github secret`__ with the name of TIPI_AUTH.

.. _github_secrets_link: https://docs.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets

__ github_secrets_link_

.. _authentication: 07-authentication.rst

.. image:: tipi-ci.png
   :alt: Add tipi auth in github secrets

