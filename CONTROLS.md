# CONTROLS.md — Buttplug.io Device Provider

This document describes every control that can appear in a device's Toy Panel, how it maps to device capabilities, and what physical effect each control produces.

Toy Panel controls are generated dynamically from each device's reported Buttplug capabilities (`device.messageAttributes`). Not all controls appear for every device — only those the device actually supports.

---

## Control Types

### Intensity Slider — ScalarCmd (Vibrate)

**Appears when:** `device.messageAttributes.ScalarCmd` contains an actuator with `ActuatorType: "Vibrate"`

**UI label:** "Intensity" (or actuator label if the device has multiple actuators)

**Range:** 0–100% (maps to Buttplug scalar `0.0–1.0`)

**Physical effect:** Controls motor vibration speed. At 0% the motor is off. At 100% it runs at full speed.

**Multi-actuator devices:** Devices with multiple vibrate actuators (e.g., Lovense Edge with tip motor and base motor) get one slider per actuator, labeled by actuator index or name.

---

### Intensity Slider — ScalarCmd (Oscillate)

**Appears when:** `device.messageAttributes.ScalarCmd` contains an actuator with `ActuatorType: "Oscillate"`

**UI label:** "Oscillation"

**Range:** 0–100% (maps to Buttplug scalar `0.0–1.0`)

**Physical effect:** Controls oscillation speed. Behavior varies by device — typically a back-and-forth or up-and-down motion distinct from vibration.

---

### Constrict Slider — ScalarCmd (Constrict)

**Appears when:** `device.messageAttributes.ScalarCmd` contains an actuator with `ActuatorType: "Constrict"`

**UI label:** "Constrict"

**Range:** 0–100% (maps to Buttplug scalar `0.0–1.0`)

**Physical effect:** Controls air bladder inflation or mechanical constriction. At 0% fully deflated/released. At 100% fully inflated/constricted.

**Safety:** Start at low values. See [SAFETY.md](SAFETY.md) for constriction device precautions.

---

### Inflate Slider — ScalarCmd (Inflate)

**Appears when:** `device.messageAttributes.ScalarCmd` contains an actuator with `ActuatorType: "Inflate"`

**UI label:** "Inflate"

**Range:** 0–100% (maps to Buttplug scalar `0.0–1.0`)

**Physical effect:** Controls air inflation. Distinct from Constrict in that inflation affects size/pressure rather than grip.

---

### Rotate Slider — ScalarCmd (Rotate)

**Appears when:** `device.messageAttributes.ScalarCmd` contains an actuator with `ActuatorType: "Rotate"`, and the device does **not** support `RotateCmd`

**UI label:** "Rotation Speed"

**Range:** 0–100% (maps to Buttplug scalar `0.0–1.0`)

**Physical effect:** Controls rotation speed. Direction is fixed (device-dependent).

---

### Bidirectional Rotation — RotateCmd

**Appears when:** `device.messageAttributes.RotateCmd` is defined

**UI controls:** Speed slider (0–100%) + Direction toggle (clockwise / counter-clockwise)

**Range:** Speed `0–100%` maps to Buttplug `0.0–1.0`. Direction is a boolean.

**Physical effect:** Controls rotation speed and direction. Lovense Nora is the most common example — the rotating head can spin either direction at variable speed.

**Implementation note:** Internally sends `device.rotate(speed, clockwise)` via Buttplug `RotateCmd`.

---

### Position Slider — LinearCmd

**Appears when:** `device.messageAttributes.LinearCmd` is defined

**UI controls:** Position slider (0–100%) — duration is internally managed

**Range:** 0% = one end of stroke, 100% = other end of stroke. Maps to Buttplug linear `position: 0.0–1.0`.

**Physical effect:** Commands the device to move to a specific position in its stroke range. The device moves at its default speed (duration parameter, fixed at 500ms in v1.0).

**Safety:** Ensure physical clearance before use. See [SAFETY.md](SAFETY.md) for linear device precautions.

**Devices:** The Handy, OSR-2, and other linear actuators.

---

### Battery Status Indicator

**Appears when:** `device.messageAttributes.SensorReadCmd` contains a sensor with `SensorType: "Battery"`

**UI:** Read-only indicator showing battery level (0–100%)

**Physical effect:** None — display only.

**Update frequency:** Queried on device connect and periodically during session (Host-controlled interval).

---

### Activity Status Indicator

**Appears when:** Always present for connected, approved devices.

**UI:** Connected / disconnected indicator.

**Physical effect:** None — display only. Reflects the Buttplug client's connection state for that device.

---

## Multi-Actuator Device Examples

### Lovense Lush 3 (single vibrate actuator)

| Control | Type | Range |
|---|---|---|
| Intensity | Intensity Slider (Vibrate) | 0–100% |
| Battery | Battery Indicator | 0–100% |

### Lovense Edge 2 (dual vibrate actuators)

| Control | Type | Range |
|---|---|---|
| Tip Intensity | Intensity Slider (Vibrate, actuator 0) | 0–100% |
| Base Intensity | Intensity Slider (Vibrate, actuator 1) | 0–100% |
| Battery | Battery Indicator | 0–100% |

### Lovense Nora (vibrate + rotate)

| Control | Type | Range |
|---|---|---|
| Vibration | Intensity Slider (Vibrate) | 0–100% |
| Rotation Speed | Bidirectional Rotation (RotateCmd) | 0–100% + direction |
| Battery | Battery Indicator | 0–100% |

### The Handy (linear)

| Control | Type | Range |
|---|---|---|
| Position | Position Slider (LinearCmd) | 0–100% |

### Lovense Max 2 (vibrate + constrict)

| Control | Type | Range |
|---|---|---|
| Vibration | Intensity Slider (Vibrate) | 0–100% |
| Constrict | Constrict Slider | 0–100% |
| Battery | Battery Indicator | 0–100% |

---

## Control Availability Notes

- Controls only appear for capabilities the device actually reports. If a control is missing, the device doesn't support that command.
- The `ActuatorType` labels in this document come from the Buttplug.io spec. The UI may use friendlier display names.
- Multi-actuator sliders are labeled by actuator index (0, 1, 2...) in v1.0. Human-readable actuator names (e.g., "Tip", "Base") are a planned improvement.
