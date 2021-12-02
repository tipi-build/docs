---
title: Authentication
order: 5
---

# Authentication

Both your local _tipi_ CLI installation as well as your tipi.build cores and nodes require access to your tipi
account to be able to access private repositories and your subscription information.

In order to enable a frictionless usage _tipi_ comes with a crendetials store dubbed _tipi vault_ which is essentially a zero-knowledge encrypted storage linked to your account.

## Creating of the vault

During the onboarding on [tipi.build](https://tipi.build) you will be asked to create said _vault_ and to provide a
passphrase for it. That passphrase is used during the browser session to encrypt the vault and is never sent to our servers.

In the following onboarding steps you will be given the opportunity to grant your account access to private
repositories on Github.com which is required if you want to consume privatly listed dependencies. 

That access can be granted at any time using the [vault dashboard on tipi.build](/onboarding/step2).

## Authenticating to tipi.build with `tipi` CLI

Run the `tipi connect` command and follow the instructions. 
You will be prompted with a link to authenticate the device on tipi.build. After confirming the access to your vault the CLI will ask for your _vault passphrase_.

> For private cloud and on premise users: you can connect to your private deployment of tipi.build by specifying the `TIPI_ENDPOINT` environment variable

## Authentication in Continuous Integration context

On non-interactive usages of `tipi` credentials can be provided by setting the following environment variables: `TIPI_ACCESS_TOKEN`, `TIPI_REFRESH_TOKEN`, `TIPI_VAULT_PASSPHRASE`.

## Authentication with a Personal Access Token on Github

You can add a Personal Access Token to your vault. This allows you to have access to the directories of an organization that has not yet accepted tipi-build as an application 

To do this :
    - go on [tipi.build](https://tipi.build)
    - Go to the page from your vault and unlock your vault
    - Click on add credentials

    You must have this page now 

<div class="columns">
  <div class="column is-10">
    <content-img-figure src="./assets/add-credentials.png">
      Credentials add page
    </content-img-figure>
  </div>
</div>

    - Fill in all the fields and click on save.
    - Now with the `tipi` CLI use `tipi connect`
