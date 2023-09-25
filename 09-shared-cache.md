---
title: Share build cache across my team
aliases: []
---

By default all tipi users use their private, independent build cache.
For a few dependencies they will benefit from a curated build cache from `tipi-build` hosted on GitHub that includes short list of projects.

Provided all team members have access to your `my-organisation` group on GitHub and all machines export the `TIPI_POWWOW` environment variable you will then share the build cache:

## Testing cache with one build

During setup we recommend that you run a build like this from your machine:

```
TIPI_POWWOW=my-organisation time tipi -t linux-cxx17 build .
```

Afterwards run the same command on a coworkers machine and enjoy the speedup.
