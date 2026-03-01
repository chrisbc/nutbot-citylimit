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

### Tested — Session 2 (2026-02-27)

| Parameter | Value | Result |
|---|---|---|
| Wire | 1.6mm 316 stainless welding rod | Workable material |
| Hole size | 1.5mm in timber batten | Tight fit, difficult to drill consistently |
| Tip | 3mm female bullet connector (crimped) | **FAILED** — too long, insufficient capture |
| Spacing | 15mm centres, 4×5 array | **Promising** — fingers spread around nuts correctly |
| Test surface | Grass + hard surface, real acorns | Failure consistent on both |

**Conclusions:**
- **Spacing (15mm) is approximately correct** — fingers spread around acorns as intended;
  this variable can be considered provisionally validated
- **Tip geometry and wire tension are the failure modes** — bullet connector is too long,
  and wire lacked sufficient spring force to close over and retain the acorn securely
- **Manufacturing is a hard blocker regardless of scale** — drilling 1.5mm holes and pressing
  in wire requires fine dexterity and causes hand strain even for a small batten; this method
  is not viable for DIY construction
- If DIY drum continues: tip design and assembly method both need a rethink before further builds

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

## Alternative: BagaNut Collection Wheels

Commercial proven wheels rather than DIY drum. Eliminates finger geometry development
and manual assembly entirely.

### BagaNut Wheel Options (for acorns)

| Code | Colour | Fits | Est. price |
|------|--------|------|------------|
| F0031 | Light green | Pecan, large acorns, hazelnuts, crabapples, almonds | $11–$58 |
| F0033 | Blue | **Small acorns**, joba bean, coffee bean | $11–$58 |

Plan: buy 4× F0031 + 4× F0033 to evaluate geometry and fit across acorn size range.

### Reverse Engineering / 3D Print Investigation

Scanning and printing replacement or modified wheels for personal use is a viable
cost-reduction path. Not for commercial reproduction.

**Scanning options:**
- Photogrammetry (free — Polycam, Scaniverse, Meshroom) — adequate for body geometry,
  may struggle with thin tines
- Structured light scanner (e.g. Revopoint, ~$500–2000) — much better for fine tine detail
- Strategy: buy one wheel first, scan it, then decide on full set purchase

**Printing material — critical choice:**

The tines must flex over acorns and spring back thousands of times without fatigue failure.
Layer lines from FDM printing are a known weak point when oriented perpendicular to flex direction.

| Material | Flex fatigue | Outdoor UV | Print method | Notes |
|----------|-------------|------------|--------------|-------|
| TPU | Good | Fair | FDM (desktop) | Best desktop option for flexible tines |
| SLS Nylon (PA12) | Excellent | Good | SLS (print service) | Closest to injection-moulded — recommended |
| PETG | Poor | Fair | FDM (desktop) | Will crack at tine roots under repeated flex |
| PLA | Very poor | Poor | FDM (desktop) | Not viable outdoors |

*FDM = Fused Deposition Modeling — standard desktop 3D printing (filament melted and deposited layer by layer)*

**Recommended path:**
1. Purchase 1× F0033 wheel (~$11–15) to scan and physically evaluate
2. Scan with photogrammetry first; use structured light if tine detail is insufficient
3. Print test copy in TPU on desktop printer — evaluate flex and fatigue
4. If TPU inadequate, get SLS nylon quote via print service (Craftcloud, JLCPCB, Shapeways)
5. Compare per-unit print cost vs purchase price before committing to full print strategy

**Economics:** Printing only makes sense if you need many replacements or want to iterate
on tine geometry (e.g. adjust spacing for your specific acorn size). At $11/wheel the bar
is low — evaluate honestly after test print.

## Open Items

- [ ] Confirm drum diameter vs ground clearance with tractor geometry
- [ ] Determine drum height adjustment mechanism
- [ ] Size hopper for realistic daily collection volume
- [ ] Comb tine spacing — determine after test rig results
- [ ] Implement wheel choice (castors vs small pneumatic)
