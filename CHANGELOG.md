# Changelog — PlayRooms-DP-Buttplug

All notable changes to this project will be documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
This project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.0.0] — 2026-03-08

### Added

- **Provider scaffolding (Milestone 1)** — initial repository structure extracted from PlayRooms Host v3.3.0
- `src/client.ts` — Buttplug client connection, device event handling, command dispatch, and scanning lifecycle (copied from Host)
- `src/engine.ts` — Intiface Engine process lifecycle management: start, stop, restart, hardware transport detection (copied from Host)
- `src/device-approval.ts` — Device approval/deny/reset/forget workflow with persistent storage, global device settings (maxIntensity, allowedCommands, displayName), and stale device cleanup (copied from Host)
- `src/protocol-filter.ts` — Application-layer protocol allowlist: regex pattern matching for 17 device brands (Lovense, Hismith, We-Vibe, Kiiroo, Satisfyer, and more) (copied from Host)
- `src/index.ts` — Placeholder provider export with identity constants (`PROVIDER_NAME`, `PROVIDER_VERSION`, `PROVIDER_API_VERSION`)
- `package.json` — Provider package identity, `buttplug ^3.2.2` dependency, TypeScript build config
- `tsconfig.json` — TypeScript compiler config targeting ES2022/NodeNext
- `manifest.yaml` — Provider manifest: identity, settings schema (intifacePort, scanTimeout, logLevel), AI interaction policy, risk flags (insertion-depth/medium, constriction/medium, noise-level/low)
- `README.md` — Supported device categories, connection methods, setup instructions, protocol allowlist documentation
- `SAFETY.md` — Emergency stop behavior (stopAll/stopDevice), connection loss handling per manufacturer, intensity ranges, hardware-specific precautions (vibration, linear, constriction, e-stim)
- `CONTROLS.md` — All panel control types: intensity slider (vibrate/oscillate/constrict/inflate/rotate), bidirectional rotation (RotateCmd), position slider (LinearCmd), battery indicator, activity indicator; with multi-actuator examples (Lovense Lush, Edge, Nora, Max, The Handy)
- `NOTICE.md` — Third-party attributions (buttplug BSD-3-Clause, drizzle-orm Apache-2.0, TypeScript Apache-2.0, Intiface Engine BSD-3-Clause)
- `qa/v1.0.0-buttplug-provider-scaffold.md` — QA checklist for Milestone 1 scaffolding

### Notes

- ProviderInterface implementation is pending (Milestone 4)
- Source files in `src/` retain their original import paths from the Host repo — these will be resolved in Milestone 4 when the provider is decoupled from Host internals
---

*Earlier history belongs to the PlayRooms Host repository (HAButtPlugIO-PlayRooms through v3.3.0).*
