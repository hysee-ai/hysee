# Install

Current platform: **macOS 13 Ventura or newer, Apple Silicon**.

[中文](install.zh-CN.md)

## Download the app

1. Open [https://hysee.ai](https://hysee.ai).
2. Download the macOS Apple Silicon installer (`HYSEE-<version>-arm64.dmg`).
3. Drag **HYSEE.app** into `/Applications`.

Get the installer from the product site. This repository does not host release binaries.

## First launch on macOS

HYSEE is ad-hoc signed. Gatekeeper may block the first open.

- Right-click **HYSEE.app** → Open → confirm, or
- Clear the quarantine flag:

```bash
xattr -cr /Applications/HYSEE.app
```

## First agent

Creating or starting an agent downloads the guest rootfs if it is not already cached. Files land in:

```text
~/Library/Application Support/HYSEE/
```

That cache is persistent. Later agents reuse it. Launching the app with no running vault does not download the rootfs.

You need a working network the first time an agent kind is provisioned (CLI bits install into the shared agent-store on first start). After that, a vault can be started from cache.

## What you need

- Mac with Apple Silicon
- macOS 13 Ventura or newer
- Disk space for the cached rootfs (on the order of a Linux userland, not the slim dmg)

You do **not** need Homebrew, Docker, or a local Go toolchain to run the app.

## Homebrew

A Cask formula exists in the product packaging (`hysee`, asset name `HYSEE-<version>-arm64.dmg`). The public tap is not the primary install path yet. Until a tap is published, download from [hysee.ai](https://hysee.ai).

## Linux and Windows

Not available as downloadable installers today. The runtime design has a Linux Firecracker path and a Windows path on the roadmap.

## Remove

Delete **HYSEE.app**, then optionally:

```text
~/Library/Application Support/HYSEE
~/Library/Logs/HYSEE
~/Library/Preferences/ai.hysee.desktop.plist
```
