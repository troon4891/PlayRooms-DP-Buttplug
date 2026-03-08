# PlayRooms — Buttplug.io Device Provider

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A [PlayRooms](https://github.com/troon4891/PlayRooms) device-provider plugin that integrates **100+ intimate hardware devices** via [Buttplug.io](https://buttplug.io) and [Intiface Engine](https://intiface.com/engine/).

---

## What This Provider Does

This plugin bridges PlayRooms and the Buttplug.io protocol ecosystem:

1. **Manages Intiface Engine** — starts and monitors the engine process bundled by the Host
2. **Discovers devices** — scans via Bluetooth LE, Serial, and USB HID (configured per-host)
3. **Filters by protocol** — application-layer allowlist prevents unrecognized devices from appearing
4. **Device approval workflow** — new devices appear as "pending" until the room owner approves them
5. **Dynamic panel schemas** — each device gets a custom control panel built from its reported capabilities
6. **Executes commands** — translates PlayRooms control events to Buttplug device calls
7. **Emergency stop** — immediately stops all devices or a specific device on demand

---

## Supported Device Categories

| Category | Examples | Control Type |
|---|---|---|
| Vibrators | Lovense Lush, We-Vibe Sync, Satisfyer | Intensity slider (0–100%) |
| Rotating devices | Hismith machines, Lovense Dolce | Speed + direction |
| Linear / strokers | The Handy, OSR-2 | Position slider |
| Dual-motor devices | Lovense Edge, Lovense Nora | Two independent intensity sliders |
| Inflation / constriction | Lovense Max | Constrict slider |
| USB HID devices | Kiiroo KEON, OhMiBod | Intensity slider |

For the full device compatibility list, see the [Buttplug.io device configuration](https://github.com/buttplugio/buttplug-device-config).

---

## Connection Methods

Devices connect through Intiface Engine using one of three transports:

- **Bluetooth LE** — most wireless consumer devices (Lovense, We-Vibe, Satisfyer, etc.)
- **Serial / Lovense Dongle** — Lovense USB Dongle, serial-connected devices
- **USB HID** — Kiiroo, some OhMiBod devices

Transports are enabled or disabled in the PlayRooms Host configuration. The Host must have the appropriate hardware (Bluetooth adapter, USB dongle) for each enabled transport.

---

## Setup

This provider is loaded automatically by the PlayRooms plugin loader. It does **not** ship its own Intiface Engine binary — the Host bundles the engine and starts it before this provider connects.

**Prerequisites:**
- PlayRooms Host v1.0.0 or later
- Intiface Engine bundled with the Host (no separate installation needed)
- At least one supported transport enabled in Host configuration

**To connect a device:**
1. Enable the appropriate transport in Host settings (Bluetooth, Serial, or HID)
2. Open the PlayRooms Settings → Devices tab
3. Click **Start Scan**
4. Put your device in pairing mode (see your device manufacturer's instructions)
5. When the device appears, click **Approve** to allow it to be controlled
6. The device will now appear in the ToyBox for room assignment

---

## Protocol Allowlist

By default, all recognized Buttplug protocols are enabled. You can disable specific brands/protocols in Settings → Devices → Protocol Filter to prevent those devices from appearing in discovery.

Devices that don't match any known protocol pattern are shown as "unknown" — you can still approve or deny them manually.

---

## AI Interaction

This provider enables AI interaction by default (`aiInteraction.defaultEnabled: true`). AI participants in a room can control devices using vibration, rotation, and linear commands. The Host enforces a 75% intensity cap on AI-driven commands.

AI control can be disabled per-device at the room level in PlayRooms room settings.

---

## Safety

See [SAFETY.md](SAFETY.md) for emergency stop behavior, connection loss handling, and hardware-specific safety notes.

---

## Extracted from PlayRooms Host

This provider was extracted from the [PlayRooms](https://github.com/troon4891/PlayRooms) Host repo at v3.3.0. The `src/` directory contains the original Buttplug client, engine lifecycle, device approval, and protocol filter code. Full ProviderInterface implementation is in progress (Milestone 4).
