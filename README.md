# Unetic shared GitHub workflows

Reusable CI, OpenWrt packaging, release, and opt-in development deployment for
the Unetic repositories.

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
- `Deploy development build` is manual and accepts only the `main` branch. It
  builds on a GitHub-hosted runner, then sends only the artifact to the LAN
  runner. The LAN runner never checks out repository code.

## Development router deployment

Create a Linux self-hosted runner on a trusted machine with LAN access to the
router. Follow the repository's **Settings → Actions → Runners → New
self-hosted runner** commands, add the `unetic-deploy` label during
configuration, and install the runner as a service. Do not register this runner
in a public runner group that permits pull-request workflows.

In each application repository, create a GitHub environment named
`development`. Restrict it to `main`, optionally require a reviewer, and add:

- `OPENWRT_HOST`: router hostname or address;
- `OPENWRT_USER`: normally `root`;
- `OPENWRT_SSH_KEY`: a dedicated private key whose public half is authorized on
  the router;
- `OPENWRT_KNOWN_HOSTS`: the verified host-key line for that router.

Start `.github/workflows/deploy.yml` from the Actions tab. The workflow copies
the APK to `/tmp`, installs it with APK's development-only
`--allow-untrusted` flag, removes it even when installation fails, and restarts
only `unetic-core`. Web and CLI deployments do not restart services or reboot
the router.
