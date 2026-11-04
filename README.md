<!--
SPDX-FileCopyrightText: The packages Authors
SPDX-License-Identifier: 0BSD
-->

# packages

The package definitions [breto](https://github.com/metio/breto) installs from. Each `NAME.edn` file defines the package `NAME`: where its releases come from, which asset each platform downloads, and how breto verifies it. Each `@NAME.edn` file defines a group of packages.

breto reads it by default. A configuration that sets `:registries` replaces that default, so add other registries under `:extra-registries` to keep reading this one:

```clojure
{:extra-registries [{:github "metio/toolchains"}]}
```

Every push to `main` that changes a definition publishes a release: `packages-VERSION.tar.gz` with the definitions and their licenses, its `packages_VERSION_SHA256SUMS`, and the cosign bundle of that checksum file, signed by the release workflow. breto verifies the bundle before it reads the registry.

## Adding a package

Add `NAME.edn` and check it with `breto validate .`, which checks every definition and group on every platform without downloading anything. The [breto README](https://github.com/metio/breto#package-definitions) describes the keys of a definition.

## Package tests

A definition can list `:tests`, commands that must exit 0 once the package is installed, such as `kind`'s:

```clojure
:tests [{:args ["kind" "version"] :expect "v{version}"}]
```

The Verify workflow runs `breto test` for every definition a pull request adds or changes, on a Linux x86_64, Linux aarch64, macOS x86_64 and macOS aarch64 runner. It installs each package from the definitions in the pull request and runs its tests; a package without a release for a runner's platform is skipped there, and one without tests passes once it installs. Running the workflow by hand tests the packages it is given. The [breto README](https://github.com/metio/breto#tests) describes the keys of a test.

## Definition checks

A scheduled workflow runs `breto check --all` every week against the latest release of every definition on every platform and lists the failures in its summary. It makes about one GitHub API request per definition, so a run spends a couple of hours waiting out the rate limit of the workflow's token.

## License

[0BSD](LICENSES/0BSD.txt).
