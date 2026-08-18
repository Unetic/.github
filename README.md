# Unetic GitHub

Shared GitHub Actions workflows for the
[Unetic](https://github.com/Unetic) repositories.

This repository keeps common CI and OpenWrt packaging logic in one place so
individual projects only need small workflow wrappers.

## Workflows

### Rust CI

Used by `unetic-core` and `unetic-cli`.

Runs formatting checks, Clippy, tests and release builds.

### Web CI

Used by `unetic-web`.

Runs dependency installation, formatting checks, tests and the production Angular build.

### OpenWrt packaging

Reusable workflow for building OpenWrt APK artifacts with official OpenWrt SDKs.

Production signed package repositories are published separately by
[`Unetic/packages`](https://github.com/Unetic/packages).

## License

GPL-2.0-only.
