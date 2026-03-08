# NOTICE

## PlayRooms-DP-Buttplug

Copyright 2024-2026 troon4891

Licensed under the MIT License (see LICENSE file).

---

## Source Code Origin

The files in `src/client.ts`, `src/engine.ts`, `src/device-approval.ts`, and
`src/protocol-filter.ts` were extracted from the PlayRooms Host repository
(https://github.com/troon4891/PlayRooms) at version 3.3.0 under the same
copyright and license.

---

## Third-Party Dependencies

### Runtime Dependencies

| Package | Version | License | Description |
|---------|---------|---------|-------------|
| buttplug | ^3.2.2 | BSD-3-Clause | Buttplug.io client library for device communication |

### Peer Dependencies (provided by Host)

| Package | Version | License | Description |
|---------|---------|---------|-------------|
| drizzle-orm | ^0.38.0 | Apache-2.0 | TypeScript ORM (used by device-approval.ts) |

### Development Dependencies

| Package | Version | License | Description |
|---------|---------|---------|-------------|
| typescript | ^5.7.0 | Apache-2.0 | Type-safe JavaScript compiler |

---

## Bundled Binary

Intiface Engine is **not** bundled with this provider. It is bundled and managed
by the PlayRooms Host. See the Host repo for Intiface Engine attribution.

Intiface Engine is developed by Nonpolynomial Labs, LLC and is available at
https://github.com/intiface/intiface-engine under the BSD-3-Clause license.
