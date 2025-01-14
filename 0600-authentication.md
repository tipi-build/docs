---
title: Authentication
aliases: [ "04-authentication", "06-authentication" ]
---

For `cmake-re` you can authenticate with your **tipi vault** you get a secure storage for - among other things - the access tokens to your private (GitHub) repositories, 
meaning that tipi instances on machines you linked to your account get to access privates repositories (that you authorized).

During the on-boarding on [tipi.build](/) you will be asked to create said _vault_ and to provide a passphrase for it. That passphrase is used during the browser session to encrypt the vault and is never sent to our servers.

In the following on-boarding steps you will be given the opportunity to grant your account access to private repositories on GitHub.com which is required if you want to consume privately listed dependencies. 

That access can be managed at any time using the [vault dashboard on tipi.build](/dashboard/vault).

## Connect your machine to your account

To _pair_ a tipi installation with your tipi account use the `connect` verb and follow the instructions:

```sh
$ tipi connect
tipi needs your tipi.build account to work remotely, authorize this machine by visiting:

         https://tipi.build/dashboard/tokens/validate?exchange=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx


         Pairing code : NNNNNN
```

After granting access to your vault in your tipi dashboard the `tipi` cli will ask for your _passphrase_ after which your machine will be able
to decrypt the vault and the pairing process is completed.

> For private cloud and on premise users: you can connect to your private deployment of tipi.build by specifying the `TIPI_ENDPOINT` environment variable

### Re-Pair your machine

In case you need to reset the settings on your machine you can do a `tipi connect --clean`.

Both your local _tipi_ CLI installation as well as your [tipi.build](/) cores and nodes require access to your tipi
account to be able to access private repositories and your subscription information.



## Authentication in Continuous Integration context

On non-interactive usages of `tipi` credentials can be provided by setting the following environment variables: `TIPI_ACCESS_TOKEN`, `TIPI_REFRESH_TOKEN`, `TIPI_VAULT_PASSPHRASE`.

These are available in files at the root of the tipi folder.

> The tipi folder is either `/usr/local/share/.tipi/` on Unix platforms or `C:\\.tipi` on Windows.

The content of these variables should be set as follow. : 
  - Content of the `.access_token` file as `TIPI_ACCESS_TOKEN`  environment variable
  - Content of the `.refresh_token` file as `TIPI_REFRESH_TOKEN` environment variable
  - `TIPI_VAULT_PASSPHRASE` plain as environment variable

## Authentication with a Personal Access Token on Github

tipi.build grants access to your repositories automatically during the on-boarding. 

However if you want to grant different access level to repositories from an organization, from another account or that weren't authorized yet to use tipi.build you can add a [GitHub Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) to your vault. 

The personal access token are secured by the vault and are only used by you on your local tipi builds or by the short-lived remote build instances. 


1. Create a [GitHub Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
2. Open your [tipi.build secure vault](/dashboard/vault)
3. Unlock the vault (this happens in your browser, nothing is transmitted to tipi.build)
4. Add your Personal Access Token credentials by adding an additional `https://github.com` or any GitHub Enterprise endpoint.

![Add credentials screenshot](./assets/add-credentials.png)

Now with the `tipi` CLI you can refresh your authentication data with `tipi connect`.
