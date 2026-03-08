# SAFETY.md — Buttplug.io Device Provider

This document covers the physical safety behaviors of this provider: emergency stop, connection loss handling, intensity limits, and hardware-specific precautions.

---

## Emergency Stop

### `stopAll()`

Calls `device.stop()` on every device currently connected to the Buttplug client, in parallel. Each stop call is fire-and-forget — errors on individual devices are logged but do not prevent other devices from receiving the stop command.

Devices that support the Buttplug `StopDeviceCmd` message (which is all modern Buttplug-compatible devices) will go to zero intensity immediately. The Buttplug protocol guarantees `StopDeviceCmd` support for every device it enumerates.

**Timing:** Stop commands are sent the moment the call is made. Network latency to Intiface Engine (loopback) is sub-millisecond. Device firmware response time varies — most BLE devices respond within 100–300ms.

### `stopDevice(deviceId)`

Sends `device.stop()` to a single device by its Buttplug index. Same behavior as `stopAll()` but scoped to one device.

### What "stop" means per command type

| Device command type | Effect of stop |
|---|---|
| ScalarCmd (vibrate, oscillate, constrict, inflate) | Scalar value set to 0.0 |
| RotateCmd | Rotation stopped |
| LinearCmd | Device halts at current position (does not return to 0) |

**Important for linear devices:** `StopDeviceCmd` on a stroker/linear device halts motion at the current position. It does not drive the device back to the zero/home position. If your device has a physical home position, you may need to command it to position 0.0 before stopping.

---

## Connection Loss Behavior

### Intiface Engine crash or disconnect

If Intiface Engine crashes or the WebSocket connection to it drops:

- The Buttplug client fires a `disconnect` event
- All device connections are considered lost immediately
- Devices **will continue at the last commanded state** until their own firmware timeout

There is no automatic reconnection in v1.0. The Host must restart the provider (or restart Intiface Engine) to restore control.

### BLE device firmware timeouts

When a BLE device loses its connection to Intiface Engine, most devices have built-in safety timeouts. However, behavior varies significantly by manufacturer:

| Brand | Observed timeout behavior |
|---|---|
| Lovense | ~10 seconds at last intensity, then stops automatically |
| We-Vibe | Stops immediately on connection loss |
| Satisfyer | Varies by model — some stop immediately, some continue |
| Hismith | Continues until manually stopped or powered off |
| The Handy | Stops automatically on WebSocket disconnect |
| Other/unknown | Behavior unpredictable — assume it may continue |

**Recommendation:** Never leave a connected device unattended. Always use the emergency stop before closing a PlayRooms session.

### Wi-Fi devices (Lovense via Lovense Connect app)

Devices connected through the Lovense Connect app over Wi-Fi (rather than direct BLE) go through an additional relay layer. Connection loss behavior depends on both the Lovense Connect app's timeout logic and the device firmware. Behavior may differ from direct BLE connections.

---

## Intensity Ranges

Buttplug normalizes all intensity values to the range `0.0–1.0` for ScalarCmd commands. This maps to the following in the PlayRooms panel UI:

- `0.0` → 0% (off)
- `0.5` → 50%
- `1.0` → 100% (maximum)

The Host enforces a per-device maximum intensity setting (`maxIntensity` in device global settings, default `1.0`). AI-controlled commands are further capped at 75% by the Host.

---

## Hardware-Specific Safety Notes

### Vibration devices (vibrators, oscillators)

- Running at maximum intensity for extended periods (30+ minutes) can cause device overheating
- Most devices have built-in thermal protection, but it is good practice to allow rest periods at high intensities
- Noise level increases significantly above 70% intensity on most vibration devices

### Linear / stroker devices (The Handy, OSR-2, etc.)

- Ensure adequate physical clearance around the device before starting a session
- The LinearCmd `position` value (0.0–1.0) maps to the full physical range of the device's stroke
- Do not obstruct the device's motion path while it is operating
- See "Emergency stop — linear devices" above for stop behavior at position

### Constriction / inflation devices (Lovense Max, air bladder toys)

- Start at very low inflation values (5–10%) when first using the device
- Constriction devices should never be used at maximum values without prior familiarization
- If using AI control with constriction devices, consider lowering the `maxIntensity` device setting to a safe maximum for your comfort level

### e-Stim devices

This provider supports Buttplug.io's e-stim protocol if the device is Buttplug-compatible. However, dedicated e-stim support is handled by the DG-LAB provider plugins (`PlayRooms-DP-DGLabs-*`), which have e-stim-specific safety controls. Using this generic Buttplug provider for e-stim is not recommended unless you understand the device's behavior at Buttplug intensity values.

---

## Known Limitations

1. **Intiface Engine dependency** — if the engine process crashes, all device connections drop simultaneously. There is no automatic recovery in v1.0.
2. **BLE range** — Bluetooth LE range is typically 10–30 meters depending on the environment. Signal loss at range will cause connection loss (see above).
3. **Device firmware quirks** — some devices have documented firmware bugs in the [Buttplug.io device configuration](https://github.com/buttplugio/buttplug-device-config). Check the device config for known issues with your specific device.
4. **Linear device homing** — `StopDeviceCmd` does not home linear devices. If your use case requires the device to return to a start position, implement this as a separate command before stopping.
5. **Multi-device stop ordering** — `stopAll()` sends stops in parallel with no ordering guarantee. All devices should stop within a few hundred milliseconds of each other.
