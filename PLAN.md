# NutBot CityLimit — Project Plan

## Overview

Autonomous outdoor mobile platform based on a recycled electric wheelchair chassis.
Primary use: collect acorns (and future: sweeping, cleaning) autonomously.
Longer term: mapping and autonomous navigation similar to the OpenMower project.

---

## Platform

| Item | Detail |
|---|---|
| Chassis | Recycled electric wheelchair, 2WD differential drive |
| Motors | 2× 24VDC brushed motor/gearbox assemblies, direct drive pneumatic tyres |
| Current | ~80A startup, 5–10A each at normal load |
| Power | 2× 12V lead-acid batteries in series (24V bus) |

---

## Phased Approach

### Phase 1 — Drive Platform & Teleoperation
- Motor controller selection and wiring
- Single board computer + ROS2
- Joystick/keyboard teleoperation
- E-stop
- Validate platform handles real outdoor terrain

**Success criteria:** Drive reliably via joystick, e-stop cuts drive instantly.

### Phase 2 — Localisation (parked)
RTK GNSS + IMU + wheel odometry → EKF sensor fusion. Deferred until Phase 1+4 validated.

### Phase 3 — Mapping & Navigation (parked)
Nav2 stack, area coverage planning (OpenMower-style). Deferred.

### Phase 4 — Acorn Collector Tool
See detail below. Being developed in parallel with Phase 1.

---

## Phase 4: Acorn Collector Design

### Mechanism Concept

Spring-finger rotating drum ("egg whisk" / Nut Wizard style).

- Rotating drum with spring wire fingers
- Fingers flex into turf, close over acorn, lift it
- Fixed comb/stripper at ~10 o'clock position strips acorns off fingers into hopper
- Wire basket/hopper below comb — removable for emptying

**Release method chosen: comb-over-basket (continuous, passive, no trigger needed)**

### Key Design Variables (to be tested)

| Variable | Range |
|---|---|
| Wire gauge | 2.0mm, 2.5mm, 3.0mm spring steel |
| Finger length | 80–150mm |
| Axial spacing | 12–25mm |
| Circumferential spacing | 4–8 rows |
| Approach/rake angle | -20° to +20° |
| Tip/knob type | bare J-bend, loop, welded ball, plastic bead |

### Reference Dimensions

- Acorn size: ~15–25mm dia × 20–35mm long
- Axial finger spacing target: ~12–15mm (must be < min acorn diameter)
- Drum diameter: ~220mm
- Drum width: ~350mm

### Drum Speed

Passive ground-driven (no separate drum motor) — drum rotates as robot moves forward.
Drum RPM is a consequence of forward speed, not an independent variable.
Collection effectiveness is determined by finger geometry, not drum speed.
Active drum motor deferred unless passive proves insufficient.

---

## Experimental Test Rig — "Ladder Drum"

A modular rotary test rig to evaluate finger array combinations before committing to final drum design.

### Design

- Central shaft: M16 threaded rod
- End discs: cut from 5–6mm flat plate (angle grinder)
- 6× longitudinal bars: 20×3mm flat bar, drilled with 3mm holes every 12mm along length
- Fingers: spring wire threaded through bar holes, secured by bent tip or small nut
- Bars bolted or tack-welded to end discs

### Dimensions

| | |
|---|---|
| Drum diameter | 220mm |
| Drum width | 350mm |
| Bars | 6 (start), every 60° |
| Holes per bar | ~25 at 12mm pitch |

### Push Test Frame

Simple two-arm timber or RHS frame, shaft through drilled holes or pillow blocks.
Hand-pushed over acorn test patch in grass.

### Build Sequence

1. Cut and drill end discs
2. Cut 6 bars, drill hole pattern
3. Assemble on threaded rod (no welding yet)
4. Thread first wire batch, test
5. Weld bar-to-disc joints once bar count confirmed

---

## Materials to Acquire

| Item | Use | Source |
|---|---|---|
| Galvanised garden wire 2–2.5mm | Tier 1 geometry tests | Hardware store |
| Spring steel / piano wire 2mm, 2.5mm, 3mm | Tier 2 proper tests | Engineering supplier / online |
| 25×3mm flat bar (~2m) | Test jig bars | Steel supplier |
| 5–6mm flat plate (~400×400mm) | End discs | Steel supplier |
| M16 threaded rod (~500mm) | Shaft | Hardware store |
| Pillow block bearings (2×) | Shaft support | Engineering supplier |
| Plastic beads or small balls | Tip knob trials | Craft shop |

---

## Open Questions / Decisions Pending

- [ ] Motor controller selection for main drive (wheelchair motors, 24V, 80A peak)
- [ ] Compute platform (Pi 5 vs other)
- [ ] Drum mounting and attachment interface to chassis (front mount, floating/spring-loaded)
- [ ] Hopper emptying strategy (manual vs. automated)
- [ ] Weatherproofing requirements

---

## Notes

- BBS02 Bafang 750W 48V mid-drive motor considered and rejected for drum drive —
  too complex to interface (voltage mismatch, proprietary controller, chainring coupling without lathe)
- Passive drum drive validated as correct approach — commercial nut gatherers confirm this works
- OpenMower project is the reference for future navigation stack (uses ROS2, RTK GNSS, Nav2)
