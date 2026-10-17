# Build: making, validating and cataloging brick models in code

Owner: the **Head of Build**, with the **Brick Librarian** owning the catalog and **Technical QC** running the validator. This replaces the clay studio's `FABRICATION.md`. Sources: research/04 (code pipeline, including hands-on tests on bpy 5.2.2), research/09 section 6, research/06 section 4. Items marked [U] are unverified.

## 1. Principle

Every model is an LDraw file of real parts by ID. Code writes it, the validator judges it, Blender renders it. Agents don't describe models to each other; they hand over `.mpd` files, part IDs and JSON. The best brick features worked the same way: a library keyed by part ID with connection data, and tools that made illegal builds hard (research/01 section 2).

## 2. The brick library (Brick Librarian)

- **Parts:** the official LDraw parts library, pinned by release name and SHA-256 (2026-09 at research time: 17,164 unique parts). Official parts only by default; unofficial parts need a Librarian decision. Ship a `NOTICE` with the LDraw credit, the CC BY license and the LDraw trademark line (research/04 section 1).
- **Colors:** `LDConfig.ldr` from the same release, plus the hex convention picked in look dev (DESIGN_LANGUAGE.md section 4).
- **Studs:** plain, or our house glyph as a custom stud primitive. Never logo studs. ImportLDraw defaults to plain; ldr_tools_blender defaults to a logo stud, so set it to Normal explicitly (research/04 section 1). A render check fails any frame with a logo stud mesh in the scene.
- **The catalog** (`catalog/catalog.json`, built by `studio brick catalog`):

| Field | From |
|---|---|
| part ID, name, category, keywords, official or not, license header | LDraw part headers |
| bounding box, stud positions, typed connections (studs, tubes, receptacles, bars, clips) | pyldraw3 geometry analysis (research/04 section 4) |
| pivots and joint axes (figure parts, hinges, turntables) | re-derived from part geometry; never copied from GPL data files |
| colors this part really comes in | Rebrickable part-color data (free to use, credit appreciated) |
| prints and textures | LDraw `!TEXMAP` and patterned part variants |
| house flags: allowed, exotic, retired, banned and why | Librarian decisions |

- **Banned list:** patterned parts whose name or keywords point to film or licensed characters, themes or logos, so they can't be placed even by accident (FAN_RULES.md section 3). Kept in `catalog/banned.json`, with the reason per part.
- **Library updates** run through the `library-update` workflow: pull, rebuild the catalog, re-validate every approved model, report anything that broke.

## 3. Authoring tools for agents

Agents don't type raw coordinates and hope. They use commands that only produce legal placements:

| Command | Does |
|---|---|
| `studio brick search <words> [--size 2x4] [--color 4] [--category Slope]` | Finds parts in the catalog, filtered to colors the part comes in |
| `studio brick place <part> <color> --on <model>:<part-instance>:<stud> [--rot 90]` | Places a part onto a real connection point; refuses impossible placements |
| `studio brick module <name> [params]` | Inserts a parametric module: wall, window bay, floor, stairs, roof, tree, road, vehicle chassis, figure |
| `studio brick bom <model>` | Parts list with quantities, colors and unique-part count |
| `studio brick render-quick <model>` | Fast preview from three angles (LeoCAD CLI or Workbench) for the builder's own checking |

- **Scripted designs first.** Python generators that emit LDraw for anything regular: walls of a given height and pattern, a city block, a mosaic from an image (Lab color matching to real part colors, research/08 section 3), stairs, fences. Generators are tested code, not prompts.
- **Hand-placed details second,** through `place`, for the story parts: the figure's prop, the gag, the seed part.
- **Generators as sketches only.** Mesh-to-brick conversion (the MIT mesh2brick script) and text-to-brick models produce blocky shapes from a handful of plain bricks and can't express slopes, hinges or detail (research/04 section 5). Use them for rough massing or idea generation, never as the authority on stability and never as a finished model.
- **The agent loop** follows the pattern research/06 section 4 reports working well: search parts, place by connection, validate, render a quick view, look, fix. A validator in the loop is what took frontier models from rarely valid to almost always valid.
- **Submodels** as `.mpd` `0 FILE` blocks: one per sub-assembly, so instructions, posing and builds-itself can move them as units.

## 4. The validator (code gate)

Every model, every time it changes. Output `validate.json` with each level's result and the exact failing parts. Levels adapted from research/04 section 6:

| Level | Check | Fail means |
|---|---|---|
| L0 | Parses | Broken file |
| L1 | Every part exists in the pinned library and isn't banned | Invented or off-limits part |
| L2 | Every part-color pair exists; every color is in the build bible palette | Impossible or off-palette color |
| L3 | Grid: positions on 20 LDU (X, Z) and 8 LDU (Y), rotations in 90 degree steps, except parts placed on a legal hinge, clip, bar or sideways-stud connection | Off-grid part |
| L4 | No collision beyond the stud-in-tube allowance (bounding boxes, then mesh) | Parts through parts |
| L5 | Connection graph from typed contacts (not footprint overlap) | A part that touches nothing |
| L6 | Everything connects to the base | Floating parts or groups |
| L7 | Static stability with friction (our own solver; see below) | It would fall apart |
| L8 | Budgets: piece count, unique parts, exotic parts, prints | Over budget |
| L9 | Fan scan: no logo studs, no banned parts, no banned words in model names | Fan-rules break |

- **L7 for set look and instructions; warning for display scenes.** A display diorama only needs to look right from the camera, but floating and intersecting parts (L4 to L6) always fail.
- **Our own stability solver.** The published physics check uses a commercial solver that's free only for academics, and its free fallback only checks footprint overlap between layers, which can't handle slopes or sideways building (research/04 section 6). Build a small force-and-torque program on an open solver (HiGHS or OSQP [U]) over the L5 connection graph, and test it against the published labeled dataset (research/09 section 7).
- **Proven on known-bad models.** A test suite of planted faults (a floating plate, two bricks overlapping, a red part in a color it doesn't come in, a part off the grid by 1 LDU, an overhang that would fall, a logo stud, a banned printed part). Every check must catch its own fault, and the suite runs in CI.
- **Never a vision model.** Model judges are poor at brick assembly reasoning (research/06 section 2), so buildability is never a judged dimension.

## 5. Figures

- **Fan mode** (FAN_RULES.md): standard figure parts from the library (head, torso, hips, legs, arms, hands) are allowed, with original prints only.
- **Pivots** re-derived from part geometry. Starting values from research/04 section 8 for checking: head at (0, -24, 0) LDU from the torso origin, shoulders at (plus or minus 15.4, 8, 0), hips at (0, 32, 0). Multiply by 0.0004 for metres and convert from LDraw's minus-Y-up to Blender's Z-up.
- **Rig:** one bone per rigid part, parts parented to bones, Limit Rotation constraints from `rig/joint_limits.json`, measured on Chris's figures (GOLD_SET.md). No verified licensed rig exists, so we write our own and it can carry our license (research/04 section 8).
- **Face sets** as layered decals (mouth, eyes, brows) applied through texture swaps, made by the Print and Decal Artist (MOTION.md section 2).
- **Brick-built figures** (creatures, robots, big characters) pose by sub-assembly: each `.mpd` submodel becomes a posable group with its hinge or turntable pivot from the catalog.

## 6. Prints and decals

- LDraw supports projected textures (`!TEXMAP`), and the chosen importer reads them (research/04 section 2).
- House print style: a small set of line weights, a limited ink palette drawn from real part colors, slight print wear at high points (LOOK_DEV.md section 2).
- Every print is original. Faces, torsos, tiles and stickers live in `prints/` with a sidecar naming the author and that it's original.

## 7. Instructions

- The Instructions Designer writes `steps.json`: step order, sub-assemblies, parts per step, insertion direction, camera per step. Stored back into the `.mpd` as `STEP` meta lines so any LDraw tool can read it.
- Renders use the instruction tier (flat shading with outlines). Our own page templates (ART_BOARDS.md section 4). LPub3D (GPL) is an optional separate tool only if its layouts are ever needed.
- The same `steps.json` drives builds-itself animation (MOTION.md section 5).

## 8. Scale and performance

Measured on bpy 5.2.2, Cycles CPU, a small 2 vCPU box (research/04 section 7):
- Linked objects cost about 48 KB each (100,000 parts about 5.2 GB). Geometry Nodes instances cost about 1.7 KB each (1,000,000 about 2.2 GB, 8 s per one-sample frame).
- So: boards and figure-scale scenes (under about 10,000 parts) import with ImportLDraw for its richer materials and figure parenting; large worlds use ldr_tools_blender's Geometry Nodes instancing.
- A 2x4 brick ranges from 248 vertices (low, no bevel) to over 10,000 (high, logo, bevel). Level of detail is chosen per shot in `shot.json`: hero shots high primitives with baked bevels; mid shots standard; wide shots low with no bevel.

## 9. Asset approval

A model or figure is approved when: the validator passes at the level its use requires; the Librarian signs off on parts, colors and prints; it renders correctly on the screen-test scene; and the Head of Build approves. The approval writes a manifest with the file hash, so any later change re-opens approval.

## 10. Tools policy

| Tool | License | Use |
|---|---|---|
| LDraw parts library | CC BY 2.0 (dual 2.0 and 4.0 since 2023 per an admin) | Core. Credit always. Never repackage the CC BY-ND installer. |
| ImportLDraw 1.3.0 | GPL-2.0+ | Core importer for boards; runs headless on bpy 5.2.2 (verified) |
| ldr_tools_blender 0.5.2 | MIT | Large scenes with Geometry Nodes instancing; Blender 5.1+; headless use on the pip wheel unverified [U] |
| pyldraw3 1.7.0 | GPL-3.0+ | Geometry, connections, validate, BOM; runs as a separate CLI |
| LeoCAD | GPL-2.0 | Quick previews by CLI (needs OpenGL or xvfb [U]) |
| BrickGPT mesh2brick, LegoACE | MIT | Optional sketch generators only |
| LDCad shadow library | CC BY-SA 4.0 | Optional connection data, loaded from a user path, never bundled |
| BrickLink Studio, Mecabricks | Proprietary; terms partly unread | Not in the pipeline; never ship their parts or renderers |
| Bricker | GPL, paid | Not needed |

GPL tools run as separate processes or user-installed dependencies. What that means for the plugin's own license is in ENGINEERING.md section 6.
