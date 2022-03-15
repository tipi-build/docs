---
title: Data Security and Privacy
order: 10
---

# Data Security and Privacy

## Website data
The first data tipi.build gets are from visitors and user of the tipi.build webapp. As you might have noted, tipi doesn't require submitting cookie preferences. It's because we don't use any third party cookies to track users and tipi doesn't share any user data to a separate analytics platform or third parties.

The tipi.build web platform is a fully custom implementation and does not relly on 3rd party services (except for cloud server hosting obviously). The data that we log is anonymized and used only for short-term debugging purposes.

## Authenticating registered users
We rely on external OAUTH providers like Github.com to authenticate users, and only ask access to the user name and email. We use these information and store them in our database along with a generated unique identifier to authenticate actions on the website.

## User's source code
To guarantee full data privacy and security the tipi.build cloud has no access to the user's source code outside of interactive sessions.

<div class="columns">
  <div class="column is-full ">
      <content-img-figure src="./assets/architecture-layered.drawio.png">
        tipi.build cloud : Source code is only in clear-text on the local and on the short-lived build runner instance
      </content-img>
    </figure>
  </div>
</div>

Indeed only the tipi cli tool scans user source code and it does this either locally or remotely within a short-lived build runner instance ( _i.e._ A freshly provisioned virtual machine ) started for the current build session.

The local tipi cli encrypts the source code folder as encrypted+compressed AES 256/CBC blocks that are transmitted via tipi's storage server to the short-lived build runner over a TLS connection.

The tipi storage server only **ever** sees an encrypted block, while the short-lived build runner decrypts it into an hard disk encrypted storage to run the build. 

The storage server encrypted+compressed AES 256/CBC blocks and the short lived build runner are deleted after a timeout at the end of the build session.

The short-lived remote build runner is made accessible only trough asymetric cryptography generated on each new build session, which means that only the local machine sending the source code and initiating the session has the data to access the short-lived remote build runner and the decrypted sources.

### tipi's secure vault : private repositories
On the first registration tipi.build users are asked to create a so-called *tipi vault*. The tipi.build vault is a cryptographically secure container that stores access tokens to user's private repositories hosted elsewhere ( _e.g._ on Github.com ).

These tokens are encrypted and decrypted using the user's vault passphrase by an [open source C++ client-side only software library](https://github.com/tipi-build/vault) that is run in webassembly in the browser and natively in the build nodes. The vault passphrase is never shared to the tipi.build cloud.

The vault is uploaded by the user's browser as a purely encrypted binary blob, which means that even if our always up-to-date, 24/7 monitored website gets hacked and someone steals the tipi.build database, it will find at best user emails and encrypted blob that would take him more than 20 years for today's supercomputer to bruteforce the information.

No serious attacker would actually choose this road, as the encrypted token after such a databreach would all be revoked by tipi.

<div class="columns">
  <div class="column is-full">
      <content-img-figure src="./assets/vault_management.png">
        With the tipi.build vault all authentication data is only known by the local browser or the user current machines
      </content-img>
    </figure>
  </div>
</div>

### tipi computer pairing
To use tipi.build remote compilation and execution capabilities, it is possible to locally save a vault for use on the user computer. That means if the computer is attacked or at risk, the tokens in the vault might get stolen ( they are not saved in clear-text though ). 

If an user ever suspect that his computer was subject of an attack or a data breach, it is possible to revoke access anytime and regenerate the vault and it's tokens.

<div class="columns">
  <div class="column is-full">
      <content-img-figure src="./assets/tipi_access_tokens.png">
        tipi.build : revoke local machines access tokens to the tipi.build account
      </content-img>
    </figure>
  </div>
</div>