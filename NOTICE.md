# NOTICE

## PlayRooms-DP-Buttplug

Copyright 2024-2026 troon4891

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

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
