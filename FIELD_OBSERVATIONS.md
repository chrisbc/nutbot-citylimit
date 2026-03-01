# Field Observations

## Session 1 — Manual Raking & Collection
**Date:** 2026-02-21
**Conditions:** Early fall season, lawn area

### Observations

**1. Acorn Embedment**
- Acorns on the lawn for several weeks work down into the turf and are significantly harder to dislodge
- Fresh-fallen acorns are much easier to collect
- Implication: frequent collection (weekly during fall) is more effective than infrequent deep cleaning
- Design decision: target fresh-fallen acorns — do not over-engineer for deeply embedded case

**2. Debris — Twigs and Leaves**
- Significant volume of twigs and leaves mixed in with acorns
- Both will interfere with the comb-stripper mechanism (twigs jam, leaves wrap/clog drum)
- **Design decision: accept mixed collection — nuts, twigs and leaves all collected together**
- All material will be mulched in a 1hp Hansa electric mulcher and composted
- No pre-screening, sorting, or debris deflection required
- Manual clog clearing accepted during testing phase
- Twig and branch diameter varies considerably — small twigs to substantial branches
  present on lawn at the same time; clog risk varies accordingly
- Leaves currently low volume but will increase significantly as autumn progresses
- Wet, heavy leaves are harder to collect and damage lawn if left — motivates frequent passes
- **Operational target: daily passes during peak season**
  - Keeps leaf/nut volume manageable per run
  - Prevents leaves becoming wet and matted
  - Protects lawn from smothering and acorn germination

**3. Terrain — Holes in Lawn**
- Where holes have developed, acorns fall in and are effectively lost
- Confirms drum must have floating/spring-loaded mount to follow terrain
- Accept that hole-trapped acorns are a write-off — do not engineer for this case

**4. Collection Volume**
- Gathered approx 25kg in one session from roughly 1/10th of total drop area
- This is the very start of the fall season (early this year)
- Implies total seasonal load could be very large
- Hopper/bag must handle meaningful volume per run to avoid constant emptying

---

## Session 2 — Finger Prototype Test (Flat Batten)
**Date:** 2026-02-27 (Friday)
**Conditions:** Slightly damp, overcast day
**Tester:** Chris

### Setup

- **Material:** 1.6mm 316 stainless steel welding rod
- **Finger tips:** 3mm crimp-style female bullet connectors, crimped onto rod ends
- **Batten:** Wooden batten, 1.5mm holes drilled in a 4×5 array (4 rows, 5 fingers per row)
- **Spacing:** ~15mm centres axially and circumferentially
- **Test method:** Batten pushed/rolled over real acorns on grass and on hard surface,
  simulating the rolling sweep action of a full drum

### Manufacturing Notes

- Drilling 1.5mm holes and fitting 1.6mm rod was more difficult than expected
- Assembly caused hand injury — manual manufacturing of a full drum (500+ fingers) raises
  serious feasibility concerns for hand-assembly
- Rod fit in 1.5mm holes: snug/interference fit

### Result: **FAILURE** (tip geometry and manufacturing method)

- Bullet connectors were too long — they did not reliably grip acorns
- Acorns were rarely captured; almost always fell out during the simulated rolling motion
- Tested on both grass and hard surface — failure consistent across both

### Key Learnings

- **15mm finger spacing is approximately correct** — fingers did spread around acorns as
  intended; this is a positive result for the geometry concept
- **Tip geometry is the primary failure mode** — bullet connector too long and wire lacked
  sufficient spring tension to close over and retain the acorn
- **Manufacturing method is a hard blocker** — drilling 1.5mm holes and pressing 1.6mm wire
  requires fine dexterity and caused hand strain even for this small batten; not viable for
  DIY construction regardless of scale
- The passive rolling action requires the finger tips to sweep *under* the acorn and spring
  closed — tip geometry and wire stiffness are critical and were both insufficient here

### Alternative Under Consideration

**BagaNut wheels** (commercial proven acorn/nut collection wheels):
- Established product, geometry proven in field use
- Expensive vs DIY but eliminates drum fabrication entirely
- Would save significant build time, hand wear, and iteration risk
- Worth costing and comparing against continued DIY development

---

## Reference: Commercial Acorn Removal Methods
**Source:** The Grounds Guys — "What's the Best Way to Pick Up Fallen Acorns?" (2018)

Summary of tool types and how they relate to NutBot design:

| Tool | Method | NutBot relevance |
|------|--------|-----------------|
| **Lawn sweeper** | Towed behind riding mower, **powered rotary brush** sweeps debris into hopper | Different mechanism to BagaNut — brush vs passive gripping fingers |
| **Nut gatherer / Weasel** | Rolling wire cage on a pole, acorns collected inside basket | Closest manual analogue to NutBot drum concept |
| **Leaf vacuum** | Motorised suction into bag | Alternative approach — rejected, complex, clog-prone |
| **Rake** | Manual, gathers into piles | Baseline manual method, inefficient at scale |
| **Leaf blower** | Blows into piles, then vacuum collects | Two-step; not suitable for autonomous robot |

**Key points that confirm NutBot design decisions:**
- Embedded/decomposed acorns are harder to collect — confirms frequent-pass strategy
- Leaf blower struggles with acorns partially buried in dense turf — consistent with Session 1 observations
- Lawn sweeper is the recognised mechanised solution for riding mower/tractor owners
- Acorns can be mulched — confirms chipper/compost disposal plan

**Acorn disposal options noted (for reference):**
- Mulch via chipper ✓ *(NutBot plan)*
- Compost (crush first — whole acorns take too long to break down)
- Burn
- Donate to free-range farmer

---

## Open Questions from Session 1

- [ ] How quickly do acorns embed after falling? (days vs weeks?)
- [ ] What is the total drop area in m²?
- [ ] Target collection frequency during peak fall?

---

## Session 3 — Wire Cage Collector + Lawn Observations
**Date:** ~2026-03-01
**Conditions:** Several days of use

### New Tool: Egg-Shaped Wire Cage Collector

Purchased and used extensively over several days. This is the "Nut Weasel" / Nut Wizard style
device — an egg-shaped wire cage on a pole that rolls over acorns, flexes open, captures them,
then springs closed.

**Result: Works well**
- Passive collection mechanism confirmed effective in practice
- Wire cage geometry successfully captures and retains acorns
- **Critical limitation: collection width ~100mm** — very narrow, requires many passes to cover
  an area, slow for large lawns
- Confirms the passive-grip-finger concept is valid — the geometry just needs to be scaled up

### Observations on Embedment — Critical Factor

**Acorns pushed into the lawn by foot traffic or vehicle wheels are significantly harder to
collect with any tool.** This applies to both the wire cage and would affect BagaNut wheels
and any drum design.

**Design and operational implication:** High collection frequency is critical — possibly more
important than the choice of collection mechanism. Catching acorns before they are walked or
driven over is the single biggest factor in collection effectiveness.

### Mulching — Confirmed Practical

Collected material mulched directly via chipper into compost bin, mixed with greener material
and horse manure. Works well — no issues with mixed debris (leaves, twigs, acorns together).

### Powered Rotary Brush vs Passive Finger Grip

Concern raised: BagaNut passive finger approach may be more niche and possibly less effective
than a powered rotary brush (lawn sweeper style). Both approaches are defeated by embedded nuts.
The wire cage result suggests passive grip *can* work — but the question remains whether it
scales efficiently to a wide drum at tractor speed.

**Open question:** Is a powered rotary brush a more robust collection mechanism than passive
spring fingers at scale? Worth investigating before committing to either drum design.

### Emptying — Automation Advantage of Drum Design

The wire cage must be periodically emptied by stopping, inverting/squeezing the cage, and
clearing it manually. There is no natural continuous-discharge path.

The rotary drum with fixed comb/stripper **continuously and passively discharges** into a
trailing hopper — no stopping, no operator action required during a run. This is a significant
architectural advantage for autonomous operation.

**Design decision confirmed:** Continuous passive discharge via comb-over-hopper is the right
approach for an autonomous platform. Wire cage style is not scalable to unattended operation.

## Open Questions from Session 2

- [ ] BagaNut wheel pricing — is cost justified vs DIY iteration?
- [ ] If DIY continues: what finger tip geometry would reliably sweep under an acorn? (bare bent wire? J-hook? loop?)
- [ ] If DIY continues: is there a manufacturing method for 500 fingers that doesn't require hand-drilling each hole?
