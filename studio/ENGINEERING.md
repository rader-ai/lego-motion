# Engineering: repos, pinning, shared core, CLI and tests

Owner: the **Pipeline TD**. The clay studio's `ENGINEERING.md` applies (plugin mechanics, the tracker CLI design, hooks, agents and spawning, review site, judges). This file covers what's new or different for bricks. Sources: research/04 (hands-on tests on bpy 5.2.2), research/06, research/09. Items marked [verify] or [U] get checked in Phase 0.

## 1. Repos and storage

| Repo | Visibility | Holds |
|---|---|---|
| `rader-ai/lego-motion` | Public | The plugin, studio docs, research, NOTICE. No renders, no gold photos, no A/B results. |
| `rader-ai/clay-motion` | Public | The shared operating model; cloned next to this repo |
| `rader-ai/brick-studio-rnd` (name at Phase 0) | Private | R&D ledger, tune log, A/B results, gold provenance, held-out briefs and photo twins, hidden judge config |
| `rader-ai/brick-boards` (name at Phase 0) | Private | One folder per board job (CLAUDE_MAPPING.md) |
| One repo per film | Private | As in clay |

`.blend` files through Git LFS; renders and caches never in git (gitignored `renders/`, optional bucket sync); manifests point to them by hash. Keys in an untracked `.env`.

## 2. Shared core

Chris chose a shared org, so the pieces both studios need are written once:
- the `studio` tracker CLI, packets and role checks, ledger, locks and waits;
- the hooks;
- the critic panel harness, hidden judge runner and A/B page;
- the review site;
- the animation engine's timing compiler, beat-sheet schema and secondary layers.

Phase 0 checks `../clay-motion`. If the clay studio has built any of these, Brick Studio depends on them. If not, Brick Studio builds them as a `core/` Python package with no brick-specific code in it, so it can move to its own repo (for example `rader-ai/studio-core`) and the clay studio can adopt it. Brick-specific code lives in `brick/`.

**Plugin layout**
```
lego-motion/
  .claude-plugin/plugin.json   name: brick-studio
  agents/                      one file per role (STUDIO_DELTA.md, plus shared roles)
  skills/                      studio-charter, board, make, brickify, ... (CLAUDE_MAPPING.md)
  workflows/                   board-batch, screen-test, library-update, ...
  hooks/hooks.json
  bin/studio                   shared CLI entry; brick subcommands load from brick/
  core/                        shared package (if not reused from clay)
  brick/                       catalog, authoring, validator, board layout, brick FX
  templates/boards/            HTML board templates, both formats
  catalog/                     built catalog, banned list (generated, LFS if large)
  NOTICE                       LDraw credit and license
```

## 3. Pinning and the brick smoke test

| Item | Pin (research time) | Notes |
|---|---|---|
| Blender | `bpy==5.2.2` (Blender 5.2 LTS, Python 3.13 only, about 880 MB, GPL-3.0) | Same wheel as the clay studio, so one environment can serve both |
| ImportLDraw | 1.3.0 by git SHA, on `sys.path` as `io_scene_importldraw` | Verified headless on bpy 5.2.2 |
| ldr_tools_blender | 0.5.2 release zip (Rust core, no PyPI wheel) | Blender 5.1+; headless on the pip wheel [verify] |
| pyldraw3 | 1.7.0 | Coexists with bpy in one venv (verified); needs a `parts.lst` at the library root |
| LDraw library | release name plus SHA-256 of the parts zip and `LDConfig.ldr` | ldraw.org was blocked from the research sandbox; fetch from an allowed mirror or vendor the pinned zip [verify] |
| LeoCAD | optional, for quick previews | Needs OpenGL or xvfb [verify] |
| BrickGPT, LegoACE | optional, separate venv or container | Their pins (older numpy and bpy) conflict with ours |

Install sketch: `uv venv --python 3.13 && uv pip install bpy==5.2.2 pyldraw3==1.7.0`, library path in `LDRAW_LIBRARY_PATH`, run scripts with `python -I`.

**Blender 5.2 traps** (verified in research/04 section 9), all handled in one adapter module: Geometry Nodes inputs are set through `md.properties.inputs.<id>.value` (indexing raises TypeError); the only EEVEE engine ID is `BLENDER_EEVEE`; `use_nodes` warns and goes away in 6.0; the compositor tree is `scene.compositing_node_group`; property access moved out of ID properties in 5.0; native modules must match Python 3.13.

**Brick smoke test** (SessionStart hook and CI): import a 2x4 brick (part 3001) and check its bounding box is 32 x 11.2 x 16 mm; import a transparent part, a figure and a 300-part model; confirm no logo stud mesh exists; render one Cycles CPU frame at 16 samples and diff against a golden frame.

## 4. CLI additions (brick subcommands)

| Command | Does | Refuses when |
|---|---|---|
| `studio brick catalog` | Builds the catalog from the pinned library and part-color data | Caller isn't the Librarian or Pipeline TD |
| `studio brick search / place / module / bom / render-quick` | Authoring (BUILD.md section 3) | A placement isn't on a real connection |
| `studio validate <model> [--level L7]` | Runs the validator, writes `validate.json` | n/a (it reports) |
| `studio read-test <board or render>` | Thumbnail and black-fill read test, writes `read_test.json` | n/a |
| `studio fan-scan <path>` | Scans renders, board text and file names for logos and banned names | n/a |
| `studio board new <idea> --type <t>` | Creates a board job folder from the template | Missing brief |
| `studio board render <job>` | Renders every `shot.json` in the job | Model not validated at the required level |
| `studio board compose <job>` | Fills the HTML template and exports PNG and PDF | Any render missing or failing look QC |
| `studio board final <job>` | Records Board Final | No passing validate, read test and fan scan; no Art Director approval; no Chris signature |
| `studio steps <model>` | Writes `steps.json` and STEP meta lines | Model not validated |

## 5. Hooks (additions)

| Hook | Event | Does |
|---|---|---|
| Protect the catalog | PreToolUse on Edit, Write and Bash | Denies changes to `catalog/` outside `studio brick catalog` |
| Locked models | PreToolUse on Edit and Write | Denies edits to an approved model's `.mpd` without a reopen decision |
| Brick smoke test | SessionStart | Runs section 3's smoke test |

## 6. Licenses and the GPL question

- Blender's own license guidance treats published scripts that use its Python API as GPL-bound, and several core tools here are GPL (ImportLDraw, pyldraw3, LeoCAD). LDraw parts are CC BY and need credit (research/04 section 10).
- **Option A, recommended for a fan project:** license the plugin GPL-3.0-or-later. Everything lines up, nothing has to be kept behind a process boundary, and the work can be shared freely.
- **Option B:** MIT for the plugin, with every GPL tool run as a separate process or user-installed dependency and only JSON and images crossing the boundary. More work, and whether that boundary holds is a legal question [U].
- Chris picks in Phase 0. Either way: a NOTICE file, the license ledger, and no bundled Studio, Mecabricks or share-alike data.

## 7. Compute

- Measured on a 2 vCPU box (research/04 section 7): Geometry Nodes instancing about 1.7 KB per part; linked objects about 48 KB per part; EEVEE unusable without a GPU. Use Cycles CPU for CI and previews, GPU Cycles for finals.
- Boards are stills, so a Mac or modest GPU can make them; the motion tune loop wants a real GPU (as in clay).
- Render only unique poses in motion and duplicate holds in post.
- Phase 0 measures seconds per frame for a board hero, a detail crop, an instruction step and a 2-second motion card; those numbers drive every estimate.

## 8. Review site

Shared with clay, plus a **board page**: the board at phone size and full size, its build brief, the read test, validator summary and critic notes, with two buttons (approve, note). Approving the build brief and the final board are each one tap.

## 9. Testing

- Validator: the known-bad suite (BUILD.md section 4) in CI; every check proven on its own planted fault.
- Catalog: golden checks on a few known parts (bounding boxes, stud counts, connection counts; part 3001 has 8 studs and 19 typed contacts in pyldraw3).
- Authoring: `place` never produces a model that fails L3 to L5.
- Look regression on the screen test and three approved boards.
- Board golden test: one fixed brief end to end, compared against the approved board.
- License scan: fail on any part with an unknown `!LICENSE` header.
- Nightly: the next Blender point release and the next LDraw release, so breakage shows up before it matters.

## 10. Judges

- Panel as in clay (a Claude critic, Gemini and one OpenAI model, through `studio judge`), with brick still dimensions: plastic surface and wear, part geometry and studs, miniature light and depth, scale and lens, design language and read, handmade cues. Motion adds pose and performance and stepped timing.
- **Realism pairs:** our render of a photo twin against Chris's photo of the real build, blind and order-flipped.
- **Board rubric:** 1 to 5 per rubric line (ART_BOARDS.md section 5) with anchored examples, calibrated against Chris's ratings.
- Inputs: full-resolution crops plus the whole board; studs and gaps vanish when downscaled.
- Buildability is never sent to a judge.
- Data use: check each provider's terms before sending any gold item; the private inspiration set is never sent.
