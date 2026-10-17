# Brick Studio: how it differs from the clay studio's bible

Chris chose "own repo, shared org" (Oct 17, 2026). Brick Studio runs on the clay studio's operating model, `rader-ai/clay-motion` `studio/STUDIO_BIBLE.md` v0.3: the seniority ladder, the creative and production line split, notes, the retake ladder, communication rules, accountability and escalation. This file lists what's different for bricks, and why.

Reading two bibles invites mistakes, so in Phase 2 the Producer writes one merged `STUDIO_BIBLE_BRICK.md` (the clay bible with this file applied, section 9), reviewed once at Spec Lock. Agents and skills read only the merged file.

Sources: research/01 (feature-film craft), research/09 (roles and evaluation), research/08 (brickification).

## 1. What this studio is for

Original brick-built **art boards first**, then scenes and short films, for Chris's personal use and fun, at the craft standard of the best brick-built features and brick stop-motion films. Built from real parts in code, rendered in Blender.

Two facts from the brick features shape everything here:
1. **The directors set the rule first, and every department built tools to enforce it.** Every frame had to be something you could actually build. The tools that followed made breaking that hard: legal placement checks, per-brick wear, assembly jitter, brick-built blur (research/01 section 7). Here, only the Build Validator decides whether a model can be built, and no agent can argue past it.
2. **Departments handed each other files keyed to a brick library** (part IDs, connection points, detail levels), not descriptions (research/09 section 1). Here, agents pass each other LDraw files, part IDs and JSON, and a Brick Librarian owns the catalog.

## 2. Org chart changes

```
Executive Producer (Chris)
├── Creative Director                                    (approves boards; see section 4)
├── Producer (main session)
│   ├── Production Coordinator
│   └── Pipeline TD
│       ├── Animation TD (figure rig and timing engine)
│       ├── Brick Librarian                              [new]
│       ├── Researcher
│       └── Technical QC, including the Build Validator  [new code gate]
└── Director
    ├── Head of Story (Writer, Storyboard Artist, Animatic Editor)
    ├── Art Director                                     [was Production Designer]
    │   ├── Visual Development Artist                    [now concept sketches only]
    │   ├── Board Designer                               [new]
    │   ├── Figure Designer                              [was Character Designer]
    │   ├── Build Designer                               [merges Set and World Designer with set building]
    │   └── Look Development Supervisor (Look Dev Artist)
    ├── Head of Build                                    [was Head of Fabrication]
    │   ├── Model Builder                                [was Fabricator / Rigger]
    │   ├── Print and Decal Artist                       [was Costume and Textile Artist]
    │   └── Instructions Designer                        [new]
    ├── Supervising Animator (Lead, Senior, Animator, Lip Sync Artist, Assistant)
    ├── Director of Photography (Camera and Lighting Operator)
    ├── VFX Supervisor (Compositor, Brick FX Artist)     [Effects Artist becomes Brick FX Artist]
    ├── Editor (Assistant Editor)
    ├── Sound Supervisor (Sound Designer and Dialogue Editor)
    └── Script Supervisor (Continuity)
Advisory: Critic Panel
```

## 3. Changed and new roles

Each keeps the clay bible's format (owns, decides, cannot, good looks like, fails when). Models follow `CLAUDE_MAPPING.md`.

**Art Director** (was Production Designer). Owns the production's build bible (DESIGN_LANGUAGE.md section 9): scale, palette, part vocabulary, look mode, must-name nouns. Owns every art board. Cannot approve its own boards; the Creative Director approves and Chris signs. Good: every board reads in one second and every model in it could be built. Fails when: the palette grows, scales mix without a reason, or a board looks like a known set.

**Visual Development Artist** (narrowed). In clay this role owned art boards. Here it makes labeled concept sketches and mood references only (ART_BOARDS.md step 3). Boards belong to the Art Director.

**Board Designer** (new). Lays out art boards, Set boards and instruction sheets from templates: grid, type, callouts, palette chips, parts lists, house mark. Owns the board templates. Good: clean, consistent, readable on a phone. Fails when: a layout echoes an official box or booklet, or text crowds the hero.

**Figure Designer** (was Character Designer). Designs figures from unprinted parts plus original face and torso prints: silhouette, signature color, one prop, expression set. Fails when: a figure resembles a film or licensed character.

**Build Designer** (merges set design and set building, research/09 section 3). Owns each scene's model as a parts-level file: layout on a baseplate, modules, backdrop, dressing, the story details. Good: buildable, readable silhouette, one hero scale. Fails when: detail outruns the part budget or the model only works from one angle without saying so.

**Head of Build** (was Head of Fabrication). Approves every model and figure before it's used: validator pass, Librarian sign-off on parts, colors and prints, look check on the screen-test scene. Owns `BUILD.md`.

**Model Builder** (was Fabricator / Rigger). Writes LDraw files from the Build Designer's and Figure Designer's specs using the build tools (BUILD.md section 3). Never approves its own models. Rigging is mechanical here (the engine reads pivots from the catalog), so it moves to the Animation TD.

**Print and Decal Artist** (was Costume and Textile Artist). Makes original face prints, torso prints, tile prints and sticker sheets as decal textures in the house print style. Owns the face set per figure. Fails when: a print copies a real one.

**Instructions Designer** (new). Orders a model's build into steps: sub-assemblies, step size, parts per step, camera per step. Feeds instruction sheets and builds-itself motion cards. Good: a person could follow it. Real set teams have a dedicated building-instructions role judged on natural build flow (research/09 section 2).

**Brick Librarian** (new, under the Pipeline TD). Owns the part catalog, which colors each part really comes in, part metadata (connections, pivots), detail levels, bevel, gap and jitter settings, the stud setting, the print allowlist and the banned list. Says no to any part, color or print that doesn't exist or isn't allowed.

**Build Validator** (new code gate, run by Technical QC). Checks every model and every motion frame at the right profile (BUILD.md section 4): `static` for models and sets, `posed` for figures mid-motion (no grounding or stability checks while a figure is in the air), `fx` for Brick FX parts. No agent can approve past a failed check, and vision models never judge buildability (research/06 section 2).

**Brick FX Artist** (was Effects Artist). Water, fire, smoke, dust, sparks, light beams and explosions as parts, swapped per exposure, with colors snapped to real part colors (research/01 section 5).

**Dropped:** sculpting, clay surface sign-off, armatures, costume making, replacement-face sculpting. Faces become prints.

## 4. The board crew

Art boards don't need the film crew. A board job uses: Art Director (owner), Visual Development Artist (optional sketches), Build Designer, Figure Designer if figures appear, Model Builder, Brick Librarian (data), Build Validator (code), Look Dev Artist, Camera and Lighting Operator, Board Designer, Critic Panel. The **Creative Director approves** each board and Chris signs. The Director doesn't review boards. The Producer runs tracking and spend.

## 5. Gates and locks

**Build gates** (once per studio version). Art boards come first, so the board lane gets its own gates before the motion work:

| Gate | When | Owner | Concurs | Signs |
|---|---|---|---|---|
| Spec Lock | Spec and merged bible reviewed and revised | Producer | Pipeline TD, Creative Director | Chris |
| Board Ready | Build tools, validator suites, plastic look v1, board templates, fan scan and the still-image judges calibrated | Pipeline TD | Creative Director | Chris |
| Board Bar | Board lane meets the bar (KICKOFF Phase 6) | Creative Director | Pipeline TD | Chris |
| Motion Spec Lock | Motion lane spec reviewed | Producer | Supervising Animator, Animation TD | Chris |
| Motion Ready | Figure rig, timing engine, motion QC, Brick FX tools and the motion judges calibrated | Pipeline TD | Creative Director | Chris |
| Studio Bar | Motion meets the bar on held-out shots and with outside raters | Creative Director | Pipeline TD | Chris |

**Board locks** (every board job): Brief > Build Brief (`build_bible.yaml`, one tap) > Build (validated model, Head of Build approves) > Board Final (Creative Director approves, Chris signs). Chris can record a **standing waiver** once per board type (`brick decide --type waiver`) to let the Build Brief proceed on the Art Director's call when he says "just make it". The waiver is a recorded decision and can be withdrawn.

**Film locks:** the clay bible's eight (Script, Design, Animatic, Voice, Look, Picture, Sound, Final). Design Lock adds: every model and figure passes the validator, `build_bible.yaml` is locked, and the fan scan passes.

## 6. Shot cycle changes

- **Layout:** the set loads from its validated model or a saved scene; moving or adding parts re-runs the validator.
- **Pop-through:** key poses also pass the `posed` validator profile and the brick motion checks (MOTION.md section 6) before review.
- **Final:** rendered per unique pose (holds duplicated in post, as in clay), no motion blur ever.
- **Retake reasons** add `unbuildable`, `off-palette` and `fan-rules`.

## 7. The bar, in brick terms

- **Buildability** is never judged. It's a code gate.
- **Realism:** twin realism against Chris's photos and open realism against random gold photos, on four dimensions that can differ: plastic surface and wear, part geometry and studs, miniature light and depth, handmade cues (ENGINEERING.md section 10).
- **Board quality:** same-brief comparisons during tuning; outside raters at the Board Bar.
- Motion adds pose and performance and stepped timing.
- All thresholds are our own proposals to validate, not industry standards. The exact Board Bar is in KICKOFF Phase 6.

## 8. Who can say no (additions)

| Who | Says no to |
|---|---|
| Brick Librarian | A part, color, print or part-color pair that doesn't exist or isn't allowed |
| Build Validator (code) | Anything unbuildable, deforming, unconnected or over budget |
| Art Director | Anything off the build bible: palette, scale, design language |
| Creative Director | Anything that breaks FAN_RULES.md, backed by the fan scan; any board it won't approve |

## 9. Clay rules the merged bible must rewrite

These clay lines stay wrong for bricks if copied unchanged:
- **Visual Development Artist "owns art boards"**: here the Art Director owns boards (section 3).
- **DP "fails when" cinematic CG lighting, telephoto compression or sky lighting**: the film look is cinematic on purpose, the `daylight` preset is sun and sky, and the 100 mm macro is allowed for box-art angles (LOOK_DEV.md sections 5 and 6). Rewrite the DP's failure list around fake tilt-shift, wrong scale cues and lens choices made without a reason.
- **The RACI**: add rows for Build Brief and `build_bible.yaml`, model validation, the catalog, print approval, scenes, board final and the fan scan; rename the Production Designer and Fabrication columns.
- **Motion QC "near-straight spine" warning**: a rigid figure trips it on every pose. Drop it, along with every squash and stretch check (MOTION.md section 6).
- **Material bible, maquettes, boil, fibers**: replaced by the build bible, model approval, wear and jitter.
- **Shot-only tracking**: board jobs are a job type with their own locks (ENGINEERING.md section 2).
