# Engineering: repos, pinning, shared core, CLI and tests

Owner: the **Pipeline TD**. The clay studio's `ENGINEERING.md` design applies (plugin mechanics, the tracker CLI's packets and refuse rules, hooks, agents and spawning, review site, judges) except where this file differs. Sources: research/04 (hands-on tests on bpy 5.2.2), research/06, research/09. Items marked [verify] or [U] get checked in Phase 0.

## 1. Repos and storage

| Repo | Visibility | Holds |
|---|---|---|
| `rader-ai/lego-motion` | Public | The plugin, studio docs, research, NOTICE. No renders, no gold photos, no A/B results, no built catalog. |
| `rader-ai/clay-motion` | Public | The sister kit; its bible and design docs are the shared operating model |
| `rader-ai/brick-studio-rnd` (name at Phase 0) | Private | R&D ledger, tune log, A/B results, gold provenance, held-out briefs and builds, hidden judge config |
| `rader-ai/brick-boards` (name at Phase 0) | Private | One folder per board job (CLAUDE_MAPPING.md) |
| One repo per film | Private | As in clay |

- `.blend` files through Git LFS. Renders and caches never go in git (gitignored `renders/`, optional bucket sync); manifests point to them by hash. Keys live in an untracked `.env`.
- **Repo name.** Chris kept `lego-motion` knowing the fan policy (FAN_RULES.md section 2). The fan scan allowlists git metadata (remote URL, repo name) and nothing else. The plugin, marketplace entry, binaries, skills and every output use brick names only.
- **Config.** Phase 0 writes `~/.config/brick-studio/config.toml` with the paths to the boards repo, the R&D repo, the shared Blender environment and the LDraw library, so `/brick-studio:board` knows where jobs go.

## 2. Shared core

Chris chose a shared org, so the pieces both studios need get written once: the tracker (jobs, packets and role checks, ledger, locks, waits), the hooks, the critic panel harness and hidden judge runner, the A/B page and review site, and the animation engine's timing compiler and beat-sheet schema.

How, decided in Phase 2 (not Phase 0):
- `core` is its own Python package and repo (for example `rader-ai/studio-core`), pinned by version in each plugin's environment. An installed plugin can't rely on a sibling folder, so nothing loads from `../clay-motion` at run time.
- Its tracker models **jobs** generically: a job has a type (board, shot, asset), a lock list and its own statuses, so board jobs and film shots both fit. Clay's shot cycle becomes one job type.
- Each plugin ships its **own entry point**, so two enabled plugins never put the same binary on PATH: `brick` here, `studio` in clay. Both call into `core`.
- Skills that both studios use (critic panel, dailies, sound) are copied into each plugin with a version line at the top, because plugins can't share skills.
- With Chris's OK, a one-line pointer to `studio-core` goes into the clay kit's ENGINEERING.md, so whichever studio is built first, the other adopts the same core.

**Plugin layout**
```
lego-motion/
  .claude-plugin/plugin.json   name: brick-studio
  agents/                      one file per role (from the merged brick bible, STUDIO_DELTA.md section 9)
  skills/                      studio-charter, board, make, brickify, ... (CLAUDE_MAPPING.md)
  workflows/                   board-batch, screen-test, library-update, ...
  hooks/hooks.json
  bin/brick                    entry point; core commands plus brick commands
  brick/                       authoring, validator, board layout, look, brick FX
  templates/boards/            HTML board templates, both formats
  NOTICE                       LDraw and Rebrickable credits, disclaimer
```
The catalog, the fan-scan terms list and any downloaded data live in the plugin data folder on Chris's machine, not in the repo.

## 3. Pinning and the brick smoke test

| Item | Pin (research time) | Notes |
|---|---|---|
| Blender | `bpy==5.2.2` (Blender 5.2 LTS, Python 3.13 only, about 880 MB installed per research/04 section 9, GPL-3.0) | One shared environment at the path in `config.toml`, so the clay studio can use the same one |
| ImportLDraw | 1.3.0 by git SHA, on `sys.path` as `io_scene_importldraw` | Verified headless on bpy 5.2.2 |
| ldr_tools_blender | 0.5.2 release zip (Rust core, no PyPI wheel) | Blender 5.1+; headless on the pip wheel [verify] |
| pyldraw3 | 1.7.0 | Coexists with bpy in one venv (verified); needs a `parts.lst` at the library root |
| LDraw library | release name plus SHA-256 of the parts zip and `LDConfig.ldr` | ldraw.org was blocked from the research sandbox; fetch from an allowed mirror or vendor the pinned zip [verify] |
| LeoCAD | optional quick previews | Needs OpenGL or xvfb [verify] |
| BrickGPT, LegoACE | optional, separate venv or container | Their pins (older numpy and bpy) conflict with ours |

- Install sketch: `uv venv --python 3.13 && uv pip install bpy==5.2.2 pyldraw3==1.7.0`, library path in `LDRAW_LIBRARY_PATH`, scripts run with `python -I`.
- **Importer settings, pinned:** ImportLDraw with `realScale` 1.0, `addGaps` on with `gapWidthMM` 0.15 (its default is 0.1), `bakeBevels` on with `bevelWidth` set for the look's 0.12 to 0.20 mm edges, `useLogoStuds` off, `instanceStuds` off, `useTextures` on. ldr_tools with stud type Normal (its default is a logo stud). Imported materials are replaced by the house shaders (LOOK_DEV.md section 2).
- **Blender 5.2 traps,** handled in one adapter module. Verified in research/04 section 9: Geometry Nodes inputs are set through `md.properties.inputs.<id>.value` (indexing raises TypeError); the only EEVEE engine ID is `BLENDER_EEVEE`; `use_nodes` warns and goes away in 6.0. From release notes, not tested: the compositor tree is `scene.compositing_node_group`; property access moved out of ID properties in 5.0; native modules must match Python 3.13.
- **Smoke test, full** (Phase 0 and CI): import part 3001 with gaps off and check its bounding box is 32 x 11.2 x 16 mm; import a transparent part, a figure and a 300-part model with the pinned settings; confirm no logo stud primitive exists; render one Cycles CPU frame at 16 samples and diff it against a golden frame; test whether Workbench renders headless on the render machine.
- **Smoke test, light** (SessionStart): the shared Blender check plus one brick import. Hook timeouts are short [verify].

## 4. CLI (`brick`)

Core commands from the shared package (`brick decide`, `brick note`, `brick ledger`, `brick wait`, `brick judge`, `brick report`, packets and statuses) keep the clay design. Brick commands:

| Command | Does | Refuses when |
|---|---|---|
| `brick catalog` | Builds the catalog in the data folder | Caller isn't the Librarian or Pipeline TD |
| `brick search / place / module / bom / render-quick` | Authoring (BUILD.md section 3) | No real connection; the model's manifest is approved and not reopened |
| `brick validate <model> --profile static\|posed\|fx` | Runs the validator, writes `validate.json` | n/a (it reports); fails on a hash that doesn't match an approved manifest |
| `brick read-test <render>` | Read test on the unlabeled hero render (DESIGN_LANGUAGE.md section 2) | No `must_name` list in `build_bible.yaml` |
| `brick fan-scan <path>` | The three-layer scan (section 5) | n/a |
| `brick board new <idea> --type <t>` | Creates a board job in the boards repo from the template | No brief |
| `brick board render <job>` | Renders every `shot.json` in the job | A model not validated at its profile |
| `brick board compose <job>` | Fills the HTML template, exports PNG and PDF | A render missing or failing look QC |
| `brick board final <job>` | Records Board Final | No passing validate, read test and fan scan; no Creative Director approval; no Chris signature (or his recorded standing waiver for that board type) |
| `brick steps <model>` | Writes `steps.json` and STEP meta lines | Model not validated |
| `brick motion-card <job> --kind turntable\|build` | Renders a turntable or a builds-itself card on stepped keys | Model or `steps.json` missing |

## 5. Hooks and the fan scan

| Hook | Event | Does |
|---|---|---|
| Protect state | PreToolUse on Edit, Write, Bash | As in clay, for tracker, ledger, locks, decisions and handoff notes |
| Protect the catalog | PreToolUse on Edit, Write, Bash | Denies changes to the catalog outside `brick catalog` |
| Locked models | PreToolUse on Edit, Write and Bash | Denies writes to any `.mpd` whose manifest is approved, including redirects, `sed -i`, `mv`, `cp` and generator scripts that target it |
| Brick smoke test | SessionStart | The light smoke test (section 3) |

**The fan scan** has three layers, each proven on planted faults in CI:
1. **Scene graph:** no logo stud primitive, no banned or non-allowlisted printed part, every print points to `prints/`, official-set overlap result present.
2. **Text:** board JSON, file and folder names, captions and titles checked against the banned-terms list. The list is data in the plugin data folder, read only by the scan, never quoted in skills or agent text. Git metadata is the one allowlisted exception (section 1).
3. **Pixels:** OCR on every render, board and file in `concept/`, flagging any wordmark or garbled brand text (image models invent them, research/06 section 2).

## 6. Licenses

- Blender's license guidance treats published scripts that use its Python API as GPL-bound, and several core tools here are GPL (ImportLDraw, pyldraw3, LeoCAD). LDraw parts are CC BY and need credit (research/04 section 10).
- **Option A (default):** license the plugin GPL-3.0-or-later. It fits a non-commercial fan project, pyldraw3 can be imported as a library, and nothing has to hide behind a process boundary.
- **Option B:** MIT for the parts of the plugin that never import bpy or a GPL library, with the Blender-side code as a separate GPL sub-package and GPL tools run as separate processes. Whether that boundary holds is a legal question [U].
- Chris confirms in Phase 0. Either way: NOTICE with LDraw and Rebrickable credits, the license ledger, no committed catalog, no bundled Studio, Mecabricks or share-alike data.

## 7. Compute

- Measured on a 2 vCPU box (research/04 section 7): Geometry Nodes instancing about 1.7 KB per part, linked objects about 48 KB per part, EEVEE unusable without a GPU. Cycles CPU for CI and previews, GPU Cycles for finals. Workbench headless gets tested in Phase 0; the instruction look and thumbnails fall back to low-sample Cycles if it fails.
- Boards are stills, so a Mac or a modest GPU can make them; the motion tune loop wants a real GPU (as in clay).
- Motion renders only unique poses and duplicates holds in post.
- Phase 0 measures seconds per frame for a board hero, a detail crop, an instruction step and a motion card. Phase 2 turns those into cost and time per board and per tune round, and the tune loops' round counts and time limits come from that (KICKOFF Phase 6).

## 8. Review site

Shared with clay, plus a **board page**: the board at phone size and full size, its build brief, the read test, a validator badge and critic notes, with approve and note buttons. Approving the build brief and the final board are one tap each.

## 9. Testing

- Validator: the known-bad and known-good suites (BUILD.md section 4) in CI.
- Catalog: golden checks on a few known parts (part 3001 has 8 studs and 19 typed contacts in pyldraw3; its bounding box as in section 3).
- Authoring: `place` and every module never produce a model that fails L3 to L6 under the static profile.
- Fan scan: planted faults for each layer.
- Look regression on the screen test and three approved boards (LOOK_DEV.md section 10).
- Board golden test: one fixed brief end to end, compared against the approved board.
- License scan: fail on any part with an unknown `!LICENSE` header.
- Nightly: the next Blender point release and the next LDraw release, so breakage shows up early.

## 10. Judges

Panel: a Claude critic, Gemini and one OpenAI model, through `brick judge`. Gemini counts on every still dimension (the clay limit came from video frame sampling, which doesn't apply to stills); "2 of 3 judge families" means 2 of these 3, each meeting the bar on its own.

| Test | What's compared | Dimensions scored | Used for |
|---|---|---|---|
| **Twin realism** | Our render of a photo twin against Chris's processed photo of the real build, blind and order-flipped | Only what can differ: plastic surface and wear, part geometry and studs, miniature light and depth, handmade cues | Look loop and Board Bar |
| **Open realism** | A board hero render against a random gold photo, clay-style | The same four | Proves the look holds outside the twin setup |
| **Same-brief A/B** | Candidate board against best-so-far, and against a frozen v1 baseline, on the same brief, order-flipped | Read, design language, plastic, story, layout | Design loop keep-best |
| **Rubric scores** | 1 to 5 per line with anchored examples | All lines except buildable | Logged for diagnosis only, never optimized (clay research/04) |

- Read and Story are judged on the hero render with all board text masked, so a good caption can't lift them.
- Inputs: full-resolution crops plus the whole image; studs and gaps vanish when downscaled.
- Buildability is never sent to a judge.
- Hidden judge: as in clay, run only at checkpoints from the private repo.
- Data use: check each provider's terms before sending any gold item; the private inspiration set is never sent (FAN_RULES.md section 5).
