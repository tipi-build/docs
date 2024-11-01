---
title: L1 Build Cache Sharing
aliases: [ "09-shared-cache" ]
---

By default all tipi users use their private, independent build cache.

Provided all team members have access to `my-organisation` group on GitHub and all machines export the `TIPI_POWWOW` environment variable it's possible to share a build cache.

Access rights to the repository `my-organization/cache.tipi.build` specifies the  

```
TIPI_POWWOW=my-organisation cmake-re -S . -B build/ -DCMAKE_TOOLCHAIN_FILE=environment/<toolchain-name>.cmake
```

Afterwards run the same command on a coworkers machine and enjoy the speedup.
