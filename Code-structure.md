# Code structure

The Skia Wallet's code is organized as a **monorepo** and **layer architecture**.

---

## Monorepo

We use the methodology **monorepo** (used in Google, Facebook...). It helps Skia Wallet to decouple the multiple parts of the code and to scale the project easily.

> Learn more about monorepos on this [video of Google](https://youtu.be/W71BTkUbdqE) and [Nx.dev](https://nx.dev/guides/why-monorepos).

These are the benefits of the monorepo (taken from [Nex.dev](https://nx.dev/guides/why-monorepos) and [ACM](https://cacm.acm.org/magazines/2016/7/204032-why-google-stores-billions-of-lines-of-code-in-a-single-repository/fulltext)):

- **Shared code and visibility**. Keeps your code across your entire organization. Reuse validation code, UI components, and types across the codebase. Reuse code between the backend, the frontend, and utility libraries.
- **Atomic changes**. Change a server API and modify the downstream applications that consume that API in the same commit. You can change a button component in a shared library and the applications that use that component in the same commit. A monorepo saves the pain of trying to coordinate commits across multiple repositories.
- **Developer mobility**. Get a consistent way of building and testing applications written using different tools and technologies. Developers can confidently contribute to other teams’ applications and verify that their changes are safe.
- **Single set of dependencies**. Use a single version of all third-party dependencies, reducing inconsistencies between applications. Less actively developed applications are still kept up-to-date with the latest version of a framework, library, or build tool.
- **Unified versioning, one source of truth**

You can read about Nx folder structure [here](https://nx.dev/getting-started/nx-setup)

> Hint: We don't use the folder `apps`, all the code is in the folder `packages`.

---

## Layer architecture

The code of the wallet is a **layer architecture** (specifically [Domain-Driven Design architecture](https://en.wikipedia.org/wiki/Domain-driven_design) & Clean architecture).

All layers are decoupled. How? Each one is in one or more packages in the folder `packages` of the current Github project:

- [**Common**](https://github.com/Future-Wallet/skia-wallet/tree/main/packages/common): general resources that are used on all projects.
- [**Entities**](https://github.com/Future-Wallet/skia-wallet/tree/main/packages/entities): defines how the data of the others layers will use. It contains **entities** (each entity is unique because it has at least one unique property) and **value objects** (it isn't unique). Part of the code in the entities is inspired by [Khalil Stemmler](https://khalilstemmler.com/).
  - Example of an entity: a User's Wallet, the unique properties are the mnemonic phrase, the private key or the public address.
  - Example of a value object: an address with only one property (the `value` as a type string), it can be private, public address, etc.
- [**Repositories**](https://github.com/Future-Wallet/skia-wallet/tree/main/packages/repositories): connects to external services (APIs, storage, smart contracts, blockchain providers...). It gets the data from a smart contract and transforms it to fit the layer _entities_, then the data it will be ready to be used on the layer _ui_.
- **UI**: presents to the user interface (UI) like components and the whole website. It depends on the rest of the layers. Skia Wallet has two UI packages: [ui-components](https://github.com/Future-Wallet/skia-wallet/tree/main/packages/ui-components) and [ui-wallet](https://github.com/Future-Wallet/skia-wallet/tree/main/packages/ui-wallet)
