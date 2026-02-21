# NutBot CityLimit — Tractor Platform

## Platform Summary

| Item | Detail |
|---|---|
| Base | Recycled electric wheelchair frame |
| Drive | 2WD differential drive |
| Drive wheels | ~350mm diameter, pneumatic tyres |
| Stability wheels | 4 bogey wheels, existing arrangement, accept as-is |
| Top deck | 40mm plywood, rectangular |
| Estimated weight | ~60kg (frame + batteries + deck) |
| Centre of gravity | Low — heavy steel frame and batteries |

## Batteries

- Type: sealed AGM (wheelchair spec, designed for all-day discharge)
- Arrangement: 2× 12V in series = 24V bus
- Estimated capacity: 20–40Ah (TBC when specs found)
- Estimated runtime at normal drive load: 2–3 hrs real-world

## Motors

- 2× 24VDC brushed motor/gearbox assemblies
- Startup current: ~80A per motor
- Normal operating current: 5–10A per motor
- Direct drive to pneumatic tyres

## Towing Capability

- Proven towing with 80kg trailer load
- Towing extension: timber, integral with deck, extends rearward
- Hitch point: 15mm hole through 40mm timber extension
- Hitch pin: 15mm clevis pin + R-clip (quick-release, no tools)

## Implement Interface Standard

### Mechanical
- Single pin hitch: 15mm clevis pin through timber extension
- Implement tongue: steel flat bar or tube, 15mm hole, sandwiching timber (double shear)
- Articulation: horizontal swing (turning) + vertical pitch (terrain) via pin clearance
- One implement at a time

### Electrical Umbilical (provision from day one)
- Power: Anderson SB50 connector — 24V + GND, fused per implement
- Signals: 6-pin aviation plug — PWM, GPIO, GND
- Cable: ~300mm slack loop at hitch for articulation clearance

## Payload — Compute & Nav (future phases)
- Single board computer (TBD — Pi 5 or similar)
- IMU
- RTK GNSS module
- Camera(s) / vision
- Comms (WiFi / 4G)
- Motor controller(s)
- All mounted on top deck

## Open Items
- [ ] Confirm battery Ah rating from label
- [ ] Motor controller selection (24V, handles 80A startup)
- [ ] Compute platform selection
- [ ] Top deck layout for electronics
