# NutBot CityLimit

Autonomous outdoor mobile platform for acorn collection, built on a recycled electric wheelchair chassis.

## Project Status

**Phase 1 — Drive Platform & Teleoperation**: Planning  
**Phase 4 — Acorn Collector Tool**: Parallel development, test rig design complete

---

## Quick Links

| Document | Description |
|----------|-------------|
| [PLAN.md](PLAN.md) | Project overview, phased approach, acorn collector specs |
| [TRACTOR.md](TRACTOR.md) | Wheelchair platform specs, batteries, motors, towing |
| [IMPLEMENT_NUT_COLLECTOR.md](IMPLEMENT_NUT_COLLECTOR.md) | Drum design, finger variables, test rig, materials |
| [FIELD_OBSERVATIONS.md](FIELD_OBSERVATIONS.md) | Session notes: acorn embedment, debris, terrain |
| [DESIGN_VISUALS.md](DESIGN_VISUALS.md) | SVG/OpenSCAD fabrication file workflow |

### Tractor Subsystem Docs

| Document | Description |
|----------|-------------|
| [tractor/MOTOR_CONTROLLER.md](tractor/MOTOR_CONTROLLER.md) | RoboClaw vs Sabertooth, encoder options |
| [tractor/SENSORS_AND_COMPUTE.md](tractor/SENSORS_AND_COMPUTE.md) | Perception stack, OAK-D + LiDAR, ROS2 references |
| [tractor/RC_ESC_ANALYSIS.md](tractor/RC_ESC_ANALYSIS.md) | RC ESC options, pros/cons vs RoboClaw |
| [tractor/OEM_CONTROLLER_ANALYSIS.md](tractor/OEM_CONTROLLER_ANALYSIS.md) | Reusing OEM wheelchair controller, joystick spoofing |

---

## Platform Summary

| Item | Detail |
|------|--------|
| Chassis | Recycled electric wheelchair, 2WD differential drive |
| Motors | 2× 24VDC brushed, ~80A startup, 5–10A running |
| Power | 2× 12V AGM batteries in series (24V bus) |
| Tow capacity | 80kg trailer load proven |
| Top deck | 40mm plywood, rectangular |

---

## Acorn Collector Design

**Mechanism**: Spring-finger rotating drum (Nut Wizard style), ground-driven, passive.

- Drum diameter: 220mm
- Drum width: 350mm
- Finger bars: 6 (every 60°)
- Release: Fixed comb/stripper into wire basket hopper

**Test rig**: "Ladder drum" modular design for finger geometry validation before final build.

---

## Phased Approach

| Phase | Focus | Status |
|-------|-------|--------|
| **1** | Drive platform, motor controller, teleoperation | Planning |
| **2** | Localisation (RTK GNSS + IMU + wheel odometry) | Deferred |
| **3** | Mapping & Navigation (Nav2) | Deferred |
| **4** | Acorn collector implement | Parallel development |

---

## Key Decisions

- **Passive drum**: Ground-driven via forward motion, no separate motor
- **Mixed collection**: Acorns, twigs, leaves collected together — mulched via Hansa chipper
- **Towed implement**: Drum encounters undisturbed ground before tractor wheels
- **Daily operation**: Peak season to prevent accumulation
- **ROS2 stack**: Linorobot2 as starting point (Nav2 + sensor drivers)

---

## Open Source References

### ROS2 & Navigation

| Project | Use |
|---------|-----|
| [Linorobot2](https://github.com/linorobot/linorobot2) | ROS2 differential drive template, Nav2 ready |
| [Nav2](https://github.com/ros-planning/navigation2) | Autonomous navigation stack |
| [robot_localization](https://github.com/cra-ros-pkg/robot_localization) | EKF sensor fusion |

### Perception

| Project | Use |
|---------|-----|
| [depthai-ros](https://github.com/luxonis/depthai-ros) | OAK-D ROS2 driver |
| [rplidar_ros](https://github.com/Slamtec/rplidar_ros) | RPLIDAR driver |
| [Ultralytics YOLO](https://github.com/ultralytics/ultralytics) | Custom debris detection model |

### Motor Control

| Project | Use |
|---------|-----|
| OpenMower | Architecture reference (ROS1, concepts apply) |
| [ros_roboclaw](https://github.com/sonyccd/ros_roboclaw) | RoboClaw ROS driver |

---

## Hardware Decisions (Tentative)

| Component | Selection | Notes |
|-----------|-----------|-------|
| Motor controller | RoboClaw 2x60A | Closed-loop, encoder inputs, ROS driver |
| Encoders | AS5600 magnetic + Arduino | Wheel hub mounted |
| LiDAR | RPLIDAR A2 | 2D obstacle detection |
| Camera | OAK-D | Depth + onboard ML inference |
| Compute | Raspberry Pi 5 | Nav2 + sensor fusion |
| IMU | BNO055 or MPU6050 | Terrain slope, odometry fusion |

---

## Materials to Acquire

### Test Rig (Priority)

- [ ] Galvanised garden wire 2–2.5mm (Tier 1 tests)
- [ ] Spring steel / piano wire 2, 2.5, 3mm (Tier 2 tests)
- [ ] 25×3mm flat bar (~2m)
- [ ] 5–6mm flat plate (~400×400mm)
- [ ] M16 threaded rod (~500mm)
- [ ] Pillow block bearings ×2

### Motor Controller

- [ ] RoboClaw 2x60A
- [ ] AS5600 encoders ×2 + diametric magnets
- [ ] Arduino Nano or ESP32 (encoder interface)

---

## Open Questions

- [ ] Motor controller final selection
- [ ] Compute platform (Pi 5 vs Jetson Orin Nano)
- [ ] Drum mounting attachment interface
- [ ] Hopper sizing for daily collection volume
- [ ] Custom YOLO dataset for debris detection

---

## Repository Structure

```
nutbot-citylimit/
├── README.md
├── PLAN.md
├── TRACTOR.md
├── IMPLEMENT_NUT_COLLECTOR.md
├── FIELD_OBSERVATIONS.md
├── DESIGN_VISUALS.md
└── tractor/
    ├── MOTOR_CONTROLLER.md
    ├── SENSORS_AND_COMPUTE.md
    ├── RC_ESC_ANALYSIS.md
    └── OEM_CONTROLLER_ANALYSIS.md
```