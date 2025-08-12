<!--
SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
-->

# The Beman Library Maturity Model

`The Beman maturity model` helps developers quickly assess the production readiness of Beman libraries by classifying them based on development phase and interface stability.

![](/images/beman_flow-beman_library_maturity_model.png)

### Under development and not yet ready for production use.

These libraries may deviate from the Beman Standard due to incompleteness, lack of testing, inconsistencies with the specification, or other non-conformances.

> They are not recommended for production usage!

### Production ready. API may undergo changes.

These Beman-compliant libraries are production-ready, fully implementing the target paper with complete testing and documentation. Users should be aware that future API changes are possible and that standardization is not guaranteed.

> These libraries are recommended for production usage.

### Production ready. Stable API.

These production-ready libraries offer stable, standardized APIs.  They are part of the C++ Standard and can be used as a polyfill for compilers lacking native support. Note that these libraries will be retired after two standardization cycles (6 years).

> These libraries are recommended for production usage.

### Retired. No longer maintained or actively developed.

These libraries were archived and no longer maintained. These libraries are not recommended for production use.

> These libraries are not recommended for production use!
> These libraries were removed from the Beman main distribution, but the initial authors could still support them outside the Beman Project.


Transition examples:

* They were [Production ready. Stable API.](./beman_library_maturity_model.md#production-ready-stable-api) at some point, but are no longer developed or maintained, being superseded by native compiler implementations - `Mature retirement`.

* They were [Production ready. API may undergo changes.](./beman_library_maturity_model.md#production-ready-api-may-undergo-changes) at some point, but are no longer developed or maintained, being rejected from the ISO C++ Standardization - `Early retirement`.

* They were [Under development and not yet ready for production use.](./beman_library_maturity_model.md#under-development-and-not-yet-ready-for-production-use) at some point and were abandoned - `Early retirement`.
