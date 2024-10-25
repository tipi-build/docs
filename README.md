---
title: 👋 Welcome to the CMake RE Documentation
aliases: []
---

CMake RE ( Remote Execution for CMake ) is a solution for native developers to accelerate their build and test workflows, shifting CI left to enjoy [building at the speed of their fingertips](/explore/ci-cd-at-fingertips) enabled best-in-class dual layered bulk and fine-grained caching.

`cmake-re` is a thin CMake wrapper augmenting **CMake** with remoting, caching and fine grained test parallelization. As plain CMake it embraces a flexible mindset with zero-lockin, following the motto : _if it works with plain CMake it works faster with CMake RE_.

We are regularly adding new features and publish updates on our blog and newsletter frequently. Please get in touch to learn more about our roadmap.

## Building with CMake RE
* Get started [here](/documentation/0000-getting-started-cmake)

### Supported Languages
Any language supported by default CMake is supported by CMake RE, proper
remoting and caching however requires additional customizations and
support to reap off full build performance, which we track here.

| Programming Language | Support Status |
|----------------------|----------------|
| [C & C++](/documentation/0000-getting-started-cmake)          | ✅ <span class="tag is-info">beta</span>              |
| [Rust](/documentation/0100-getting-started-rust)                 | 🦀 <span class="tag is-warning">alpha</span>           |
| Swift                | <span class="tag is-warning">alpha</span>          |

_Missing your favorite language ? You can support it with [custom environments](/documentation/0400-environments)._

### What can I do with CMake RE?
CMake RE is a suite that provides the following 4 main foundational features to achieve fast, easy and scalable Rust and C++ development:
* Containerized CMake Builds
* Build folder caching ( _CMake RE L1 Cache_ ) connected straight to sources in git repositories
  * Local to speed up you on-prem CI nodes
  * Remote &amp; shared
  * Cache as a dependency manager
* Build environments provisioning and build distribution on powerful autoscaled cloud build machines with hundreds of cores and terabytes of RAM ( _CMake RE L2 Remoting_ )
* Cross-platform building + testing on Linux, Windows, macOS or custom.

### How can I start building with Tipi in the cloud?
To get the full Tipi experience we offer our users a free trial.

### Can I build with CMake RE locally?
Yes! You can benefit from CMake RE from pure local use, guaranteeing build reproducibility with CMake RE containerized builds and CMake RE L1 local caching.

### Open Source  
We believe in the power of the community and that is why we created cmake-re free forever and we are contributors of many open source project. You can access our Open Source projects on [our GitHub organisation](https://github.com/tipi-build/).

For teams and company-wide usage we also have a commercial offer, see [Support Plans](/pricing).

<!--
### How many nines?
We love uptime, but sometimes systems fail in our complex environment. We monitor our services around the clock and will investigate any issues immediately. In case things go wrong we believe in full transparency and will always keep you up to date on [status.tipi.build](https://status.tipi.build)
-->

### 🧑‍🚀 We are here to help

💡 Tipi is soon getting out of beta! Come back regularly for updates.
We absolutely listen and act on feedback! Please get in touch at `hello@tipi.build` for any questions or to discuss your project ideas.
