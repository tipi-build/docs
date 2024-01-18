---
title: Guide to create and use customized environments
aliases: [ ]
---

## Create the docker

To create the docker that will run on the tipi server, you'll need to follow several steps.

- Step 1: install the necessary packages.

```dockerfile
USER root
RUN apt-get -y update && apt-get install -y \
  locales \
  curl \
  ca-certificates \
  build-essential \
  openssh-server \
  git \
  unzip \
  sudo
RUN locale-gen "en_US.UTF-8"
```

  In this step, you add all the tools and packages you'll need for your build (e.g. autotools).

- Step 2: installing the python package.

```dockerfile
# For Ubuntu 18.04 and 20.04
USER root
RUN apt-get -y update && apt-get install -y python 

# For Ubuntu 22.04
USER root
RUN apt-get -y update && apt-get install -y python3 
```

- Step 3: setting up the ssh service.

 ```dockerfile
USER root
RUN service ssh start
EXPOSE 22
```

- step 4: creation of users required for remote build

```dockerfile
USER root
RUN addgroup --gid 123 gh-actions-group
RUN addgroup --gid 124 wine

RUN addgroup --gid 1001 tipi && useradd --system --uid 1001 --gid 1001 -G gh-actions-group,wine,sudo --create-home --home-dir /home/tipi tipi \
  && echo 'tipi ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/tipi

RUN addgroup --gid 1000 tipi-large && useradd --system --uid 1000 --gid 1000 -G gh-actions-group,wine,sudo --create-home --home-dir /home/tipi-large tipi-large \
  && echo 'tipi-large ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/tipi-large

RUN addgroup --gid 108 tipi-rbe && useradd --system --uid 108 --gid 108 -G gh-actions-group,wine,sudo --create-home --home-dir /home/tipi-rbe tipi-rbe \
  && echo 'tipi-rbe ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/tipi-rbe

RUN mkdir -p /usr/local/share/.tipi && chown tipi:tipi -R /usr/local/share/.tipi
RUN mkdir -p /home/tipi/.ssh && chown tipi:tipi -R /home/tipi/.ssh

RUN mkdir /run/user/1001 \
 && chown tipi:tipi /run/user/1001 \
 && chmod 0700 /run/user/1001

RUN mkdir /run/user/1000 \
 && chown tipi-large:tipi-large /run/user/1000 \
 && chmod 0700 /run/user/1000

 RUN mkdir /run/user/108 \
 && chown tipi-rbe:tipi-rbe /run/user/108 \
 && chmod 0700 /run/user/108

RUN chsh -s /bin/bash root
RUN chsh -s /bin/bash tipi
RUN chsh -s /bin/bash tipi-large
RUN chsh -s /bin/bash tipi-rbe

USER tipi
WORKDIR /home/tipi
```

- step 5: install tipi

```dockerfile
USER tipi
WORKDIR /home/tipi
RUN /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/tipi-build/cli/master/install/install_for_macos_linux.sh"
```

- Step 5: Build the docker.
- Step 6: Pushing the docker into a register (docker hub or GitHub registry for example)

## Create the environment

After creating the docker, you'll need to create an environment (toolchain and a deployment file on the tipi cloud), you'll need to follow several steps.

- Step 1 : Fork this repository: https://github.com/tipi-build/environments (in private or in public)
- Step 2 : Create the deployement file:

The deployment file must contain only letters, numbers and dashes.
The file extension will be `.pkr.js`.
Since the purpose of the deployment file is to deploy a docker, the name should begin with `linux-`.
So the name of the deployment file should be something like: `linux-thenameyouwant.pkr.js`
The `thenameyouwant` must be less than 25 characters long.
example with private registry on github:

```js
{
  "variables": { },
  "builders": [
    {
      "type": "docker",
      "image": "<image_name>",
      "commit": true,
      "login" : true,
      "login_server": "ghcr.io",
      "login_username" : "<githubUsername>",
      "login_password" : "<githubPAT>"
    }
  ],
  "post-processors": [
    { 
      "type": "docker-tag",
      "repository": "<filename>",
      "tag": "latest"
    }
  ]
  ,"_tipi_version":"{{tipi_version_hash}}"
}
```

example with public registry on dockerhub:

```js
{
  "variables": { },
  "builders": [
    {
      "type": "docker",
      "image": "<image_name>",
      "commit": true
    }
  ],
  "post-processors": [
    { 
      "type": "docker-tag",
      "repository": "<filename>",
      "tag": "latest"
    }
  ]
  ,"_tipi_version":"{{tipi_version_hash}}"
}
```

If using a registry, follow the packer documentation which can be found [here](https://developer.hashicorp.com/packer/integrations/hashicorp/docker/latest/components/builder/docker).

- Step 3 : Create the toolchain file:

You need to create a cmake toolchain that uses the same name as your deployment file but with the cmake extension.
So the name of the toolchaim file should be something like: `linux-thenameyouwant.cmake`
In the repository you've forked, you'll find plenty of toolchain examples. You can also just copy, paste and rename the toolchain `linux-cxx17.cmake`

- Step 4 : Commit and push your changes

## Create the distro

Now that the environment is ready, you'll need to create a distro to use it within tipi,you'll need to follow several steps.

- Step 1 : Fork https://github.com/tipi-build/distro  (in private or in public)s
- Step 2 : You will need to modify the `distro.json` file

You must locate the part named environments and modify it as follows:

```json
{
  "name": "environments",
  "platforms": {
    "all": {
      "universal": {
        "url": "https://github.com/<your_name_or_organization>/environments/archive/<commit_id>.zip",
        "sha1": "<sha1 of the zip>",
        "root": "environments-<commit_id>"
      }
    }
  }
}
```

- Step 3 : Commit and push your change

## update your project

Now that the distro is ready, you'll need to update a little bit your project to use it ,you'll need to follow several steps.

- Step 1 : create a `.tipi` folder at the root of the project you wish to build remotely.
- Step 2 : create an `env` file in the `.tipi` folder.
- Step 3 : Fill in the `env` file as follows.

```bash
TIPI_DISTRO_JSON=https://raw.githubusercontent.com/<your_name_or_organization>/distro/<commit_id>/distro.json
TIPI_DISTRO_JSON_SHA1=<sha1 of the distro.json file>
```

## use the customized environments

Now you can use your new docker in the tipi cloud by doing this:

```bash
tipi connect
cd /path/to/your/project
tipi build . -t <filename of the toolchain> <other option from tipi that you want>
```

The first time you run this command on a new docker, it can take up to an hour.
This is normal tipi must create the environment and make it quickly available to you in the future.
Rest assured that the time will be much shorter in the future (from 10 seconds to 5 minutes for the reuse of the same basic environment (docker)).
