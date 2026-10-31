<!--
SPDX-FileCopyrightText: The packages Authors
SPDX-License-Identifier: 0BSD
-->

# packages

The package definitions [breto](https://github.com/metio/breto) installs from. Each `NAME.edn` file defines the package `NAME`: where its releases come from, which asset each platform downloads, and how breto verifies it. Each `@NAME.edn` file defines a group of packages.

List it under `:registries` in the breto configuration:

```clojure
{:registries [{:github "metio/packages"}]}
```

Every push to `main` that changes a definition publishes a release: `packages-VERSION.tar.gz` with the definitions and their licenses, its `packages_VERSION_SHA256SUMS`, and the cosign bundle of that checksum file, signed by the release workflow. breto verifies the bundle before it reads the registry.

## Adding a package

Add `NAME.edn` and check it with `breto validate .`, which checks every definition and group on every platform without downloading anything. The [breto README](https://github.com/metio/breto#package-definitions) describes the keys of a definition.

## License

The definitions whose header names Shunsuke Suzuki were converted from the [aqua registry](https://github.com/aquaproj/aqua-registry) and are [MIT](LICENSES/MIT.txt)-licensed. Everything else is [0BSD](LICENSES/0BSD.txt).
