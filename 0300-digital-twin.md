---
title: 🪩 Developer Machine Digital Twin
aliases: [ ]
---

`cmake-re` build hermeticity and remoting is based on the concept of digital twins. When a build is launched the developer-machine workspace pointed to by the source directory of cmake-re is replicated / teleported to a fresh instance of the selected [Environment](./0400-environments.md): the developer-machine digital twin.

A developer-machine digital twin isn't a full copy of the local developer machine, instead it's a short-lived instance of an hermetic environment provided with the necessary inputs for the build ( _i.e._ source code, git access tokens... ) : A freshly provisioned container or virtual machine started on demand in the user local machine or cloud account.

It is started for the current build session, individual to each user and gets the source code transferred to it directly from client build requests.

Short lived build runner get destroyed at the end of the build session and cleaned up after a configurable timeout.

The developer-pc digital twin remote build runner is made accessible only trough asymetric cryptographics keys for the SSH Protocol generated on each new build session, which means that only the local machine sending the source code and initiating the session has the data to access the short-lived local or remote build virtual environment and the decrypted sources.