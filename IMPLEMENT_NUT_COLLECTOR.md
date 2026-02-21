# NutBot CityLimit — Nut Collector Implement

## Concept

Towed passive implement. Spring-finger rotating drum picks up acorns (and mixed debris —
twigs, leaves). Fixed comb strips collected material into a removable wire basket/hopper.
All material composted via 1hp Hansa electric mulcher.

## Design Decisions

- **Passive drum**: ground-driven via forward motion, no separate motor
- **Mixed collection**: acorns, twigs and leaves accepted together — no sorting
- **Towed behind tractor**: implement encounters undisturbed ground before tractor wheels
- **Manual clog clearing**: accepted during testing phase
- **Daily operation**: during peak season to prevent leaf/nut accumulation

## Implement Frame

- Simple welded steel tube frame
- Drum mounted at front (encounters acorns first)
- Hopper/basket behind drum
- Two rear support castors or small wheels
- Steel tongue hitch to tractor (15mm hole, clevis pin)
- Umbilical connector provision near hitch (unpopulated for passive drum)

## Drum — "Ladder Drum" Design

### Key Dimensions (starting point, subject to test rig validation)

| Parameter | Value |
|---|---|
| Drum diameter | 220mm |
| Drum width | 350mm |
| Number of finger bars | 6 (every 60°) |
| Finger hole pitch | 12mm (multiple spacing options) |
| Target axial finger spacing | 12–15mm |

### Construction

- Central shaft: M16 threaded rod
- End discs: cut from 5–6mm flat plate (angle grinder)
- Finger bars: 6× flat bar 20×3mm, drilled with 3mm holes at 12mm pitch, spanning between discs
- Bars bolted to discs (weld once bar count confirmed from test rig)
- Fingers: spring steel wire threaded through bar holes

### Finger Variables (to be determined by test rig)

| Variable | Range |
|---|---|
| Wire gauge | 2.0, 2.5, 3.0mm spring steel |
| Finger length | 80–150mm |
| Axial spacing | 12–25mm |
| Circumferential spacing | 4–8 rows |
| Approach/rake angle | -20° to +20° |
| Tip/knob type | bare J-bend, loop, welded ball, plastic bead |

## Comb / Stripper

- Fixed tines at ~10 o'clock position on drum rotation
- Tine gaps slot between finger bars — fingers pass through, acorns cannot
- Material strips off fingers and falls into hopper below
- Must accommodate twig variability — may need tine spacing adjustment after testing

## Hopper / Basket

- Removable wire basket below comb
- Simple bent/welded wire construction
- Sized for a useful daily collection volume
- No screening mesh — accepts all material

## Test Rig (pre-prototype validation)

A simplified version of the ladder drum on a hand-pushed frame.
Used to validate finger geometry before committing to final implement build.

### Test rig frame
- Two timber or RHS arms
- Shaft through drilled holes or pillow block bearings
- Hand-pushed over acorn test patch in grass

### Test sequence
1. Tier 1: Garden wire + cardboard — geometry proof (30 min)
2. Tier 2: Spring steel wire + flat bar jig — material validation (2–3 hrs)
3. Tier 3: Short drum segment, hand-rolled — curved geometry confirmation

## Materials to Acquire

| Item | Use | Source |
|---|---|---|
| Galvanised garden wire 2–2.5mm | Tier 1 tests | Hardware store |
| Spring steel / piano wire 2, 2.5, 3mm | Tier 2 tests | Engineering supplier |
| 25×3mm flat bar (~2m) | Finger bars | Steel supplier |
| 5–6mm flat plate (~400×400mm) | End discs | Steel supplier |
| M16 threaded rod (~500mm) | Drum shaft | Hardware store |
| Pillow block bearings ×2 | Shaft support | Engineering supplier |
| 15mm clevis pin + R-clip | Hitch pin | Hardware store |
| Anderson SB50 connector | Power umbilical | Electrical supplier |
| 6-pin aviation plug | Signal umbilical | Electronics supplier |

## Open Items

- [ ] Confirm drum diameter vs ground clearance with tractor geometry
- [ ] Determine drum height adjustment mechanism
- [ ] Size hopper for realistic daily collection volume
- [ ] Comb tine spacing — determine after test rig results
- [ ] Implement wheel choice (castors vs small pneumatic)
