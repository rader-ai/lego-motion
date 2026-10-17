# Build: making, validating and cataloging brick models in code

Owner: the **Head of Build**, with the **Brick Librarian** owning the catalog and **Technical QC** running the validator. This replaces the clay studio's `FABRICATION.md`. Sources: research/04 (code pipeline, including hands-on tests on bpy 5.2.2), research/09 section 6, research/06 section 4. Items marked [U] are unverified.

## 1. Principle

Every model is an LDraw file of real parts by ID. Code writes it, the validator judges it, and Blender renders it. Agents pass each other `.mpd` files, part IDs and JSON rather than descriptions of models. The best brick features worked the same way, with a library keyed by part ID, connection data for every part, and tools that made illegal builds hard (research/01 section 2).

## 2. The brick library (Brick Librarian)

- **Parts:** the official LDraw parts library, pinned by release name and SHA-256 (2026-09 at research time: 17,164 unique parts). Official parts only by default; unofficial parts need a Librarian decision. The repo's `NOTICE` carries the LDraw credit, the CC BY license and the LDraw trademark line (research/04 section 1).
- **Colors:** `LDConfig.ldr` from the same release, plus the hex convention picked in look dev (DESIGN_LANGUAGE.md section 4).
- **Studs:** plain, or our house glyph as a custom stud primitive (LOOK_DEV.md section 3). Never logo studs. ImportLDraw defaults to plain; ldr_tools_blender defaults to a logo stud, so set it to Normal explicitly (research/04 section 1). The fan scan fails any scene with a logo stud primitive (ENGINEERING.md section 5).
- **The catalog** (`catalog.json`, built by `brick catalog` into the plugin data folder on Chris's machine, never committed, because it mixes LDraw data with third-party tables):

| Field | From |
|---|---|
| part ID, name, category, keywords, official or not, license header | LDraw part headers |
| bounding box, stud positions, typed connections (studs, tubes, receptacles, bars, clips) | pyldraw3 geometry analysis (research/04 section 4) |
| pivots and joint axes (figure parts, hinges, turntables) | re-derived from part geometry; never copied from GPL data files |
| colors this part really comes in | Rebrickable part-color data (terms read in 2017 per research/07 section 6 [U]; Phase 1 re-checks) |
| patterned or printed, with any `!TEXMAP` | LDraw part names, pattern IDs and texture metadata |
| house flags: allowed, exotic, retired, banned, and why | Librarian decisions |

- **Printed parts are banned by default.** Every LDraw patterned part reproduces the toy maker's print artwork, and those faces, torsos and tiles are its own artwork (research/07 section 3). The Librarian can allow a generic print (a plain clock face, a grille) with a written reason in `allowlist.json`. Figures use prints from our own `prints/` folder only (section 6).
- **Licensed and logo parts are banned outright.** Parts whose names or keywords point to film or licensed characters, themes or logos go in `banned.json` with a reason. The words that identify them live in a data file only the fan scan reads, never in skills or agent text (ENGINEERING.md section 5).
- **Library updates** run through the `library-update` workflow: pull the new release, rebuild the catalog, re-validate every approved model, and report anything that broke.

## 3. Authoring tools for agents

Agents use commands that only produce legal placements, so they never type raw coordinates by hand:

| Command | Does | Refuses when |
|---|---|---|
| `brick search <words> [--size 2x4] [--color 4] [--category Slope]` | Finds parts in the catalog, filtered to colors the part comes in | n/a |
| `brick place <part> <color> --on <model>:<instance>:<connection> [--rot 90]` | Places a part onto a real connection point | No matching connection; model approved and not reopened |
| `brick module <name> [params]` | Inserts a parametric module: wall, window bay, floor, stairs, roof, tree, road, vehicle chassis, figure, backdrop | Model approved and not reopened |
| `brick bom <model>` | Parts list with quantities, colors and unique-part count | n/a |
| `brick render-quick <model>` | Fast preview from three angles for the builder's own checking (Cycles CPU at low samples, or Workbench if Phase 0 shows it runs headless) | n/a |

- **Scripted designs first.** Python generators emit LDraw for anything regular: walls of a given height and pattern, a city block, stairs, fences, a mosaic from an image (Lab color matching to real part colors, research/08 section 3). Generators are tested code. Like `place`, they refuse to write into an approved model.
- **Hand-placed details second,** through `place`, for the story parts: the figure's prop, the gag, the seed part.
- **Backdrop modules.** Set boards and style frames need a world behind the hero, and generated plates aren't allowed on boards. So the module library includes a few brick-built backdrops (sky wall, street front, room corner, landscape step) and a plain studio sweep (ART_BOARDS.md section 2).
- **Generators as sketches only.** Mesh-to-brick conversion (the MIT mesh2brick script) and text-to-brick models produce blocky shapes from a handful of plain bricks and can't express slopes, hinges or detail (research/04 section 5). Use them for rough massing or ideas, never as a finished model or as the stability authority.
- **The agent loop** follows the pattern research/06 section 4 reports working: search parts, place by connection, validate, render a quick view, look, fix. In that benchmark, the best model without the toolchain produced a valid build for 40% of prompts; with a validator in the loop it was valid nearly every time.
- **Submodels** as `.mpd` `0 FILE` blocks, one per sub-assembly, so instructions, posing and builds-itself can move them as units.
- **No copying official sets.** The LDraw Official Model Repository holds official sets as files. Those live in a test-only folder that the authoring tools can't read, and are never used as modules or starting points.

## 4. The validator (code gate)

Every model, every time it changes. Output `validate.json` with each level's result and the exact failing parts.

| Level | Check | Fail means |
|---|---|---|
| L0 | Parses | Broken file |
| L1 | Every part exists in the pinned library; none is banned; printed parts are allowlisted or point to our own prints | Invented or off-limits part |
| L2 | Every part-color pair exists; every color is in the job's `build_bible.yaml` palette | Impossible or off-palette color |
| L3 | **Connection:** every part has at least one typed contact matched within 0.5 LDU to a neighbor (studs to tubes or receptacles, bars to clips, hinges, sideways studs) | A part that isn't attached by anything real |
| L4 | No collision beyond the allowed overlap of a connection (bounding boxes, then mesh) | Parts through parts |
| L5 | Connection graph: one connected model, or the named groups the scene declares | Floating parts or groups |
| L6 | Grounded: everything connects to the base or a declared support | Parts held up by nothing |
| L7 | Static stability with friction (our own solver; see below) | It would fall apart |
| L8 | Budgets from `build_bible.yaml`: piece count, unique parts, exotic parts, prints | Over budget |
| L9 | Fan checks: no logo stud primitives, no banned parts, prints are ours, and the official-set overlap check | Fan-rules break |
| W | Grid alignment (20 LDU in X and Z, 8 LDU in Y, 90 degree rotations) as a **warning** | Worth a look; not a fail |

- **Why grid is a warning, not a gate.** Real builds sit off a single 20 LDU grid all the time: odd- and even-width parts land half a stud apart, jumper plates offset by half a stud, and sideways building lands wherever the bracket puts it. The grid method in research/04 section 4 was the researcher's own proposal [U]. Connection is the real test.
- **Profiles** say which levels apply:
  - `static` (models, sets, board scenes): L0 to L9; L7 required for set look and instruction sheets, a warning for display scenes that only need to look right from the camera.
  - `posed` (figures and sub-assemblies mid-motion): L0 to L4, L8, L9 on every frame. L5 to L7 don't apply to a figure in the air or a part being carried.
  - `fx` (Brick FX parts): L0 to L2, L4 against non-FX parts, L9. FX parts are real parts in real colors but don't need to connect.
- **Official-set overlap (L9).** Compare the model's parts list (part and color) with official set inventories from Rebrickable's set and inventory tables (research/09 section 6). Any overlap above a threshold (start at 0.6 Jaccard [U]) goes to the Creative Director, who decides and records why.
- **Our own stability solver (L7).** The published physics check uses a commercial solver whose free license reportedly covers academic use only [U], and its free fallback only checks footprint overlap between layers, which can't handle slopes or sideways building (research/04 section 6). Build a small force-and-torque program on an open solver (HiGHS or OSQP [U]) over the L5 connection graph, and test it against the published labeled dataset (research/09 section 7).
- **Proven on both suites.** A known-bad suite of planted faults (a floating plate, two bricks overlapping, a part in a color it doesn't come in, an overhang that would fall, a logo stud, a banned part, an official print, a near-copy of an official set inventory) and a known-good suite of legal builds the checks must pass (a 1x2 on a 2x4 at a half-stud offset, a jumper offset, a sideways-stud wall, a hinged roof, a figure holding a tool). Both run in CI.
- **Never a vision model.** Model judges are poor at brick assembly reasoning (research/06 section 2), so buildability is never a judged dimension. It shows on boards as a validator badge.

## 5. Figures

- **Fan mode** (FAN_RULES.md): standard unprinted figure parts from the library (head, torso, hips, legs, arms, hands) are allowed. Every print on them is ours.
- **Pivots** re-derived from part geometry. Starting values from research/04 section 8, for checking: head at (0, -24, 0) LDU from the torso origin, shoulders at (plus or minus 15.4, 8, 0), hips at (0, 32, 0). Multiply by 0.0004 for metres and convert from LDraw's minus-Y-up to Blender's Z-up.
- **Joints:** head turn, waist turn (where the hips part allows it), shoulder swing per arm, wrist twist per hand, hip swing per leg (research/01 section 3, research/09 section 1).
- **Rig:** one bone per rigid part, parts parented to bones, Limit Rotation constraints from `rig/joint_limits.json`, measured on Chris's figures (GOLD_SET.md). No verified licensed rig exists, so we write our own (research/04 section 8).
- **Face sets** as layered decals (mouth, eyes, brows) applied through texture swaps, made by the Print and Decal Artist (MOTION.md section 2).
- **Brick-built figures** (creatures, robots, big characters) pose by sub-assembly: each `.mpd` submodel becomes a posable group with its hinge or turntable pivot from the catalog.

## 6. Prints and decals

- LDraw supports projected textures (`!TEXMAP`), and the chosen importer reads them (research/04 section 2).
- House print style: a small set of line weights, a limited ink palette drawn from real part colors, slight print wear at high points (LOOK_DEV.md section 2).
- Every print is original. Faces, torsos, tiles and stickers live in `prints/`, each with a sidecar naming its author and stating it's original. L1 and L9 check that every printed part in a model points to one of these.

## 7. Instructions

- The Instructions Designer writes `steps.json`: step order, sub-assemblies, parts per step, insertion direction, camera per step. It's stored back into the `.mpd` as `STEP` meta lines so any LDraw tool can read it.
- Renders use the instruction tier (LOOK_DEV.md section 8). Page templates are our own (ART_BOARDS.md section 4). LPub3D (GPL) is an optional separate tool only if its layouts are ever needed.
- The same `steps.json` drives the builds-itself motion card (ART_BOARDS.md section 2, MOTION.md section 5).

## 8. Scale and performance

Measured on bpy 5.2.2, Cycles CPU, a small 2 vCPU box (research/04 section 7):
- Linked objects cost about 48 KB each (100,000 parts about 5.2 GB). Geometry Nodes instances cost about 1.7 KB each (1,000,000 about 2.2 GB, 8 s per one-sample frame).
- So boards and figure-scale scenes (under about 10,000 parts) import with ImportLDraw for its figure parenting, gaps, bevels and texture support; large worlds use ldr_tools_blender's Geometry Nodes instancing. In both cases our house shaders replace the imported materials (LOOK_DEV.md section 2), and importer settings are pinned (ENGINEERING.md section 3).
- A 2x4 brick ranges from 248 vertices (low, no bevel) to over 10,000 (high, logo, bevel). Level of detail is chosen per shot in `shot.json`: hero shots high primitives with baked bevels; mid shots standard; wide shots low with no bevel.

## 9. Asset approval and locks

- A model or figure is approved when the validator passes at its profile, the Librarian signs off on parts, colors and prints, it renders correctly on the screen-test scene, and the Head of Build approves.
- Approval writes a manifest with the file's hash. From then on `place`, `module` and every generator refuse to touch it, a Bash hook blocks shell writes to it, and `brick validate` fails if the file's hash no longer matches its manifest. Changing it needs a reopen decision (`brick decide --type scope`).

## 10. Tools policy

| Tool | License | Use |
|---|---|---|
| LDraw parts library | CC BY 2.0 (dual 2.0 and 4.0 since 2023 per an admin) | Core. Credit always. Never repackage the CC BY-ND installer. |
| ImportLDraw 1.3.0 | GPL-2.0+ | Importer for boards and figure-scale scenes; runs headless on bpy 5.2.2 (verified) |
| ldr_tools_blender 0.5.2 | MIT | Large scenes with Geometry Nodes instancing; Blender 5.1+; headless use on the pip wheel unverified [U] |
| pyldraw3 1.7.0 | GPL-3.0+ | Geometry, connections, validation helpers, BOM. Imported as a library if the plugin is GPL (ENGINEERING.md section 6, Option A); a separate CLI otherwise. |
| LeoCAD | GPL-2.0 | Optional quick previews (needs OpenGL or xvfb [U]) |
| BrickGPT mesh2brick, LegoACE | MIT | Optional sketch generators only |
| LDCad shadow library | CC BY-SA 4.0 | Optional connection data, loaded from a user path, never bundled |
| BrickLink Studio, Mecabricks | Proprietary; terms partly unread | Not in the pipeline; never ship their parts or renderers |
| Bricker | GPL, paid | Not needed |
