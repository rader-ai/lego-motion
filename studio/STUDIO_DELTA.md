# Brick Studio: how it differs from the clay studio's bible

Chris chose "own repo, shared org" (Oct 17, 2026). So Brick Studio runs on the clay studio's operating model, `rader-ai/clay-motion` `studio/STUDIO_BIBLE.md` v0.3: the seniority ladder, the creative line and production line split, notes, the shot cycle, the retake ladder, communication rules, RACI and escalation all apply here unchanged. This file lists only what's different, and why. Where the two disagree, this file wins for Brick Studio.

Sources: research/01 (feature-film craft), research/09 (roles and evaluation), research/08 (brickification).

## 1. What this studio is for

Original brick-built **art boards first**, then scenes and short films, for Chris's personal use and fun, at the craft standard of the best brick-built features and brick stop-motion films. Built from real parts in code, rendered in Blender.

Two facts from the brick features shape everything here:
1. **The rule came first and tools enforced it.** The directors said every frame had to be something you could actually build, with no cheating, and every department then built tools that made breaking the rule hard: legal placement checks, per-brick wear, assembly jitter, brick-built blur (research/01 section 7). Here, buildability is a code gate that no agent can argue past.
2. **The library was the contract.** Departments handed each other files keyed to a brick library (part IDs, connection points, detail levels), not descriptions (research/09 section 1). Here, every model is a parts-level LDraw file, and a Brick Librarian owns the catalog.

## 2. Org chart changes

```
Executive Producer (Chris)
├── Creative Director                                    (unchanged)
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
    │   ├── Visual Development Artist
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

**Art Director** (was Production Designer). Owns the production's build bible (DESIGN_LANGUAGE.md section 6): scale, palette, part vocabulary, look mode, and every art board. Decides which brickification choices stand. Cannot approve its own boards; the Director approves, Chris signs. Good: every board reads in one second and every model in it could be built. Fails when: the palette grows, scales mix without a reason, or a board looks like a known set.

**Board Designer** (new). Lays out art boards, Set boards and instruction sheets from templates: grid, type, callouts, palette chips, parts lists, house mark. Owns the board templates. Good: clean, consistent, phone-readable. Fails when: layout copies an official box or booklet, or text crowds the hero.

**Figure Designer** (was Character Designer). Designs figures from parts plus original face and torso prints: silhouette, signature color, one prop, expression set. Fan mode allows standard figure parts (FAN_RULES.md). Fails when: a figure resembles a film or licensed character.

**Build Designer** (merges set design and set building, research/09 section 3). Owns each scene's model as a parts-level file: layout on a baseplate grid, modules, dressing, the story details. Good: buildable, readable silhouette, one hero scale. Fails when: detail outruns the part budget or the model only works from one angle without saying so.

**Head of Build** (was Head of Fabrication). Approves every model and figure before it's used: validator pass, Librarian sign-off on parts and colors, look check on the screen-test scene. Owns `BUILD.md`.

**Model Builder** (was Fabricator / Rigger). Writes LDraw files from the Build Designer's and Figure Designer's specs using the build tools (BUILD.md). Never approves its own models. Rigging becomes mechanical (the engine reads pivot tables from part metadata), so it moves to the Animation TD.

**Print and Decal Artist** (was Costume and Textile Artist). Makes original face prints, torso prints, tile prints and sticker sheets as decal textures in the house print style. Owns the face set per figure (mouth shapes and blinks for dialogue). Fails when: a print copies a real one.

**Instructions Designer** (new). Orders a model's build into steps: sub-assemblies, step size, parts per step, camera per step. Feeds instruction sheets and "builds itself" animations. Good: a person could follow it. Real set teams have a dedicated building-instructions role judged on natural build flow (research/09 section 2).

**Brick Librarian** (new, under the Pipeline TD). Owns the part catalog and ID map (LDraw, with Rebrickable for color availability), which colors each part really comes in, part metadata (connection points, pivots, flexible parts), detail levels, bevel, gap and jitter settings, and the logo-free stud setting. Says no to any part, color or part-color pair that doesn't exist. The closest real analogue is the features' library and tools team (research/09 section 3).

**Build Validator** (new code gate, run by Technical QC). Every model and every frame of motion passes: parts exist, colors exist for the part, parts sit on the grid at legal rotations, no interpenetration beyond stud-in-tube, connected and stable, rigid parts never deform, figure joints within limits, piece count within budget. No agent can approve past a failed check. Vision models are never used to judge buildability, because they're poor at brick assembly reasoning (research/06 section 2, research/09 section 6).

**Brick FX Artist** (was Effects Artist). Water, fire, smoke, dust, sparks, light beams and explosions as parts, animated by swapping pieces per frame, with colors snapped to real part colors (research/01 section 5).

**Dropped:** sculpting, clay surface sign-off, armatures, costume making, replacement-face sculpting. Faces become prints.

## 4. The board crew

Art boards don't need the film crew. A board job uses: Art Director (owner), Visual Development Artist (optional concept), Build Designer, Figure Designer if figures appear, Model Builder, Brick Librarian (data), Build Validator (code), Look Dev Artist, Camera and Lighting Operator, Board Designer, Critic Panel. The Director doesn't review boards; the Art Director approves and Chris signs. The Producer still runs tracking and spend.

## 5. Gates and locks

**Build gates** (once per studio version). Art boards first means the board lane gets its own gates before the motion work:

| Gate | When | Owner | Signs |
|---|---|---|---|
| Spec Lock | Spec reviewed and revised | Producer | Chris |
| Board Ready | Build tools, validator, plastic look v1, board templates and the still-image judges calibrated | Pipeline TD | Chris |
| Board Bar | Board lane meets the bar on held-out briefs (section 7) | Creative Director | Chris |
| Motion Ready | Figure rig, timing engine, motion QC, Brick FX tools and the motion judges calibrated | Pipeline TD | Chris |
| Studio Bar | Motion meets the bar on held-out shots and with outside raters | Creative Director | Chris |

**Board locks** (every board job): Brief (one tap) > Build Brief (brickify output, one tap) > Build (validated model, Head of Build approves) > Board Final (Art Director approves, Chris signs).

**Film locks:** the clay bible's eight (Script, Design, Animatic, Voice, Look, Picture, Sound, Final). Design Lock adds: every model and figure passes the Build Validator, the build bible is written, and the fan-rules check passes.

## 6. Shot cycle changes

- **Layout:** the set loads from its validated model; moving or adding parts re-runs the validator.
- **Pop-through:** key poses also pass rigid-part, joint-limit and feet-on-studs checks before review.
- **Final:** rendered per unique pose (holds duplicated in post, as in clay), no motion blur ever.
- **Retake reasons** add `unbuildable`, `off-palette` and `fan-rules`.

## 7. The bar, in brick terms

Same structure as the clay bible's Studio Bar evidence, with brick dimensions. Critic panel dimensions for stills: plastic surface and wear, part geometry and studs, miniature light and depth, scale and lens, design language and read, handmade cues (jitter, gaps, play wear). Motion adds pose and performance and stepped timing. Buildability is never a judged dimension; it's a code gate. Thresholds start at the clay bible's numbers (0.40 overall, 0.35 per dimension against gold) and are proposals to validate, not standards.

## 8. Who can say no (additions)

| Who | Says no to |
|---|---|
| Brick Librarian | A part, color or part-color pair that doesn't exist |
| Build Validator (code) | Anything unbuildable, deforming or off the grid |
| Art Director | Anything off the build bible: palette, scale, design language |
| Creative Director | Anything that breaks FAN_RULES.md, backed by a code scan for logos and banned names |
