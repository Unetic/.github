<div align="center">

# 🌐 Unetic

**Modern, unified network management for OpenWrt.**

A clean control plane and web interface for managing Wi-Fi, networks, devices,
topology and multi-node installations without exposing OpenWrt internals to the user.

[Repositories](https://github.com/orgs/Unetic/repositories) ·
[Packages](https://github.com/Unetic/packages) ·
[Releases](https://github.com/Unetic/packages/releases)

</div>

---

Unetic is an open-source management platform built on top of OpenWrt.

OpenWrt remains the networking runtime. Unetic adds a higher-level management layer
with a cleaner, consumer-friendly workflow inspired by systems such as Keenetic and UniFi.

## Components

| Repository | Purpose |
| --- | --- |
| [`unetic-core`](https://github.com/Unetic/unetic-core) | Rust control-plane daemon and ubus API |
| [`unetic-web`](https://github.com/Unetic/unetic-web) | Angular web interface |
| [`unetic-cli`](https://github.com/Unetic/unetic-cli) | Local terminal client and diagnostics |
| [`packages`](https://github.com/Unetic/packages) | Signed APK repository for OpenWrt |
| [`.github`](https://github.com/Unetic/.github) | Shared CI and release workflows |

## Install

> Unetic is under active development. The current package repository targets OpenWrt 25.12.5.

Supported package architectures:

- `aarch64_cortex-a53` — MediaTek Filogic
- `mipsel_24kc` — MT7621

On the router:

```sh
ARCH="$(cat /etc/apk/arch)"

case "$ARCH" in
    aarch64_cortex-a53|mipsel_24kc) ;;
    *)
        echo "Unsupported Unetic package architecture: $ARCH"
        exit 1
        ;;
esac

wget -O /etc/apk/keys/unetic-apk-v1.pem \
    https://unetic.github.io/packages/keys/unetic-apk-v1.pem

FEED="https://unetic.github.io/packages/25.12.5/$ARCH/packages.adb"

grep -qxF "$FEED" /etc/apk/repositories.d/customfeeds.list 2>/dev/null ||
    echo "$FEED" >> /etc/apk/repositories.d/customfeeds.list

apk update
apk add unetic-core unetic-web
```

Optional CLI:

```sh
apk add unetic-cli
```

Then open:

```text
http://<router-address>/unetic/
```

## Project status

Unetic is in early development. Current work is focused on the control-plane foundation:
transactional configuration changes, rollback, reconciliation, maintenance mode,
structured errors and a stable ubus API.

## License

GPL-2.0-only.
