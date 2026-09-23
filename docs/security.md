# Security model

HYSEE treats an agent as untrusted code that will touch files and the network. The product is a **crystal vault**: isolate first, then show the work, then gate egress.

[中文](security.zh-CN.md)

## What is in place

### Isolation

On macOS Apple Silicon the cabin is a **libkrun MicroVM**, not a container sharing the host kernel. The guest is Linux. There is no virtio-net NIC on the shipped path. Outbound HTTP(S) is proxied to a host broker.

The agent does not receive your `$HOME`. Only bind-mounted paths are visible, typically at `/work/<basename>`. Reset clears vault scratch. It never deletes the host bind.

### Visibility

The glass box is the other half of safety. Timeline, Diff, and Terminal are how you see exec, file edits, and network decisions without tailing a raw log. This is the "Safe Agents" half of the slogan: you can watch the cabin.

### Egress

Each agent has an allowlist (presets ship for Claude Code, Codex, OpenCode, Hermes, OpenClaw). Wildcards such as `*.opencode.ai` work. Unknown destinations are denied or held for an Ask decision (allow once, deny, remember).

Clipboard, guest file writes, and in-guest exec can also raise guard events depending on policy.

## Threats this is meant for

- An agent that runs `rm`, `npm install`, or shell that should not land on the host userland
- Prompt injection that tries to read secrets from the host home directory (those paths are not in the guest unless you bind them)
- An agent that tries to POST to a host you did not allow

## What this is not (yet)

Be precise. Do not read the slogan as a completed compliance suite.

| Claim people might infer | Status now |
| --- | --- |
| Hardware-isolated guest for the agent process | Yes, on macOS Apple Silicon (libkrun) |
| Host home is invisible unless you bind it | Yes |
| Default-deny egress with allowlist and Ask | Yes |
| Full DLP: redact secrets in the body, one-click allow with audit export | Not shipped as a complete product surface |
| Snapshot, hibernate, roll back live memory | Not shipped (Stop drops RAM) |
| Signed Apple Developer ID / notarization | Ad-hoc signed. Gatekeeper extra click |
| Linux or Windows installer with the same boundary | Not available as downloadable installers yet |
| Guest Docker as a nested isolation layer | Not shipped |

If you bind a folder that already contains `.env` or credentials, the agent can read that folder. The vault does not magically scrub files you chose to share.

## Operational notes

- Treat allowlists as part of the threat model. A wide `*` is a hole.
- Destroy deletes the vault. Stop only powers off the MicroVM.
- Rootfs caches live under `~/Library/Application Support/HYSEE/`. They are guest userlands, not secrets stores, but you can delete them when uninstalling.

## Related docs

- [Architecture](architecture.md)
- [Install](install.md)
