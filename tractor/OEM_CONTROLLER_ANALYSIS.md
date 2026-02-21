# OEM Wheelchair Controller Analysis

## Option: Reuse Existing Wheelchair Controller

The wheelchair already has a motor controller with joystick interface designed for human control. Reusing it could save cost and preserve safety features.

### Advantages

| Feature | Benefit |
|---------|---------|
| Already integrated | Free, proven hardware |
| Soft-start | Gentle acceleration, protects mechanics |
| Current limiting | Motor and battery protection built-in |
| Battery charging interface | Keep existing charging workflow |
| Safety features | E-stop, fault detection already implemented |
| Waterproof/rugged | Designed for outdoor use |

### Disadvantages

| Issue | Impact |
|-------|--------|
| Proprietary interface | Reverse engineering required |
| No encoder feedback | Still need separate odometry solution |
| Human-centric control | May not support precise speed commands |
| Unknown CAN/serial protocol | Documentation may not exist |

---

## Common Wheelchair Controller Brands

| Brand | Models | Interface | Notes |
|-------|--------|-----------|-------|
| **PG Drives Technology** | S-Drive, VSI, Pilot, R-Net, Q-Logic | Proprietary, some CAN | Very common, some community work |
| **Dynamic Controls** | DX, DX2, Shaman, RLI | CAN bus on DX2 | Active reverse-engineering community |
| **Curtis** | 1230, 1232, 1234, 1236 | CAN / Serial | Industrial controllers, docs sometimes available |
| **Invacare** | MK4, MK5, MK6 | Proprietary | Often OEM'd by PG Drives |
| **Pride Mobility** | Various | Varies | Often OEM'd |
| **Quickie** | Various | Varies | Often OEM'd |

---

## Possible Approaches

### 1. Joystick Spoofing (Most Likely)

Most wheelchair controllers take analog voltage from the joystick:
- Typically 0-5V or 0.5-4.5V range
- Two channels: forward/back and left/right
- Neutral = center voltage (e.g., 2.5V)

**Implementation:**
```
[Arduino] → [DAC / PWM+RC filter] → [0-5V analog] → [Controller joystick input]
[ROS /cmd_vel] → [Arduino] → [Differential mixing] → [Voltage outputs]
```

**What you need:**
- Identify joystick connector pinout
- Measure voltage range and neutral point
- Arduino with 2× analog outputs (or PWM + low-pass filter)
- Disconnect physical joystick, spoof signals

**Resources to search:**
- "wheelchair joystick arduino"
- "PG Drives VSI arduino interface"
- "Dynamic Controls DX arduino"
- "power wheelchair hack"

### 2. CAN Bus Tap

Some newer controllers use CAN bus:
- Dynamic Controls DX2 uses CAN
- PG Drives R-Net may have CAN option
- May be able to send commands directly

**Implementation:**
```
[Arduino/Teensy + CAN transceiver] → [CAN bus] → [Controller]
[ROS /cmd_vel] → [CAN commands]
```

**What you need:**
- CAN transceiver (MCP2515 or Teensy built-in)
- Protocol documentation or reverse-engineering
- Software to decode/encode CAN messages

### 3. Serial Interface

Some models have service ports:
- Curtis has programming cables
- May accept speed commands if unlocked

### 4. Pass-Through Mode

Some controllers have "attendant control" or "remote" mode:
- Designed for external joystick control
- May expose standardized interface

---

## Research Required

### Identify Your Controller

1. Find label/model number on controller box
2. Photograph connectors and PCB
3. Note any branding (PG Drives, Dynamic Controls, Curtis, etc.)

### Identify Joystick Connector

1. Open joystick housing
2. Count pins (typically 4-6 for basic, more for CAN)
3. Measure voltages with multimeter:
   - Center = neutral position
   - Forward = X channel voltage change
   - Left/Right = Y channel voltage change
4. Reverse-engineer pinout

### Search Existing Work

**Keywords:**
- `"PG Drives" arduino interface`
- `"Dynamic Controls" DX arduino`
- `power wheelchair joystick hack`
- `wheelchair controller CAN bus`
- `R-Net arduino`
- `VSI controller hack`

**Communities:**
- Open Wheelchair project: https://github.com/OpenWheelchair
- DIY electric wheelchair forums
- Raspberry Pi / Arduino forums (wheelchair projects)
- Hackaday (search "wheelchair")

---

## Existing Open Source Projects

| Project | Description |
|---------|-------------|
| [Open Wheelchair](https://github.com/OpenWheelchair) | Open source wheelchair electronics |
| [Wheelchair-Arduino](https://github.com/topics/wheelchair) | GitHub "wheelchair" topic |
| [p1_ros](https://github.com/ros-mobile-robots/p1_ros) | Permobil wheelchair ROS driver (ROS1) |

---

## Integration Architecture (If Joystick Spoofing Works)

```
                    ┌─────────────────────────────────────┐
                    │     OEM Wheelchair Controller       │
                    │  (Current limiting, soft-start,     │
                    │   safety features, charging)        │
                    └─────────────────────────────────────┘
                              ↑
                    (joystick input spoofed)
                              │
                    ┌─────────────────────────────────────┐
                    │          Arduino / Teensy           │
                    │  - Receives ROS /cmd_vel            │
                    │  - Differential drive mixing        │
                    │  - Outputs spoofed joystick voltages│
                    └─────────────────────────────────────┘
                              ↑
                         USB/Serial
                              │
                    ┌─────────────────────────────────────┐
                    │        Raspberry Pi 5 (ROS2)        │
                    │  - Nav2, sensor fusion              │
                    │  - OAK-D, LiDAR, IMU                │
                    └─────────────────────────────────────┘
                              ↑
                    ┌─────────────────────────────────────┐
                    │    AS5600 Encoders → Arduino → /odom │
                    │    (separate odometry)              │
                    └─────────────────────────────────────┘
```

**Note**: Even if joystick spoofing works, you still need encoders for odometry. The OEM controller won't provide wheel speed feedback.

---

## Next Steps

1. **Photograph your controller** — model number, branding, connectors
2. **Search for existing documentation** — forums, GitHub, Hackaday
3. **Test joystick signals** — multimeter on joystick pins while moving
4. **Report back** — determine if this path is viable before investing effort

---

## Fallback

If reverse-engineering proves too difficult or undocumented:

- Proceed with **RoboClaw 2x60A** (~$180)
- Keep OEM controller as backup
- Use OEM charging interface independently (if possible)