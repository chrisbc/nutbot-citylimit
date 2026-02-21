# Design Visuals — Options & Notes

Claude (AI assistant) can generate fabrication-ready design files directly.
Best done one component at a time once dimensions are confirmed.

## Available Formats

| Format | Best for | Opens in |
|---|---|---|
| **SVG** | 2D dimensioned drawings, to scale, printable at A3/A4 | Any browser, Inkscape (free), Illustrator |
| **OpenSCAD** | Parametric 3D models, generates STL/DXF for further use | OpenSCAD (free) |
| **DXF** (via SVG) | Flat cut patterns for plate/sheet work | FreeCAD, most CAD tools |

## Recommended Approach

- SVG dimensioned drawings for fabrication: front, side and top views with dimensions called out
- Not full engineering drawings (no weld symbols etc.) but clear enough to build from
- One component at a time, after dimensions confirmed from test rig

## Components to Draw (when ready)

- [ ] Drum end discs (flat plate, hole pattern, bolt circle)
- [ ] Finger bar (flat bar, hole pitch, length)
- [ ] Implement frame (tube sizes, overall geometry, wheel positions)
- [ ] Hitch tongue (flat bar, hole position, sandwich arrangement)
- [ ] Comb/stripper (tine spacing, mounting bracket)
- [ ] Hopper/basket (wire layout, attachment points)

## When to Use

Park until:
1. Test rig finger array results confirm drum dimensions
2. Implement frame geometry is settled
3. Ready to hand drawings to fabricator
