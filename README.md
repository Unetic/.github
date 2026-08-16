# Unetic shared GitHub workflows

Reusable CI, OpenWrt packaging, and release workflows for the Unetic
repositories.

The OpenWrt baseline is 25.12.5 using its official SDK and APK package format.
The default x86/64 target is only a CI smoke test. Core and CLI release builds
must use the SDK for the actual router target. Workflow inputs keep the target,
subtarget, SDK filename, and checksum explicit without changing package recipes.

Repositories call these workflows from very small local wrappers. Calls use
`@main` while the build system is evolving. Once its interface is stable, tag
this repository with `v1` and update callers to `@v1` so production releases do
not change when `main` changes.

Release workflows produce standalone APK files. Publishing a signed feed is a
separate next step: collect the release APKs per OpenWrt architecture, generate
an APK v3 `packages.adb` index with the OpenWrt 25.12 package tooling, sign it
with an offline-protected Unetic feed key, and publish the index, APKs, and the
public key over HTTPS.

## Events

- Pull requests and pushes run formatting, linting, tests, and release builds.
- A `v*` tag must match `Cargo.toml` or `package.json`; it runs CI, builds the
  OpenWrt package, and creates a GitHub Release containing the APK.
