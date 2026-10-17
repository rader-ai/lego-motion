# Resolution of kit review 1 (v0.1 to v0.2, Oct 17, 2026)

Every finding in `kit-review-1.md`, with the decision and a one-line reason. 37 accepted, 1 accepted in part.

## Blockers

| # | Finding | Decision | What changed |
|---|---|---|---|
| 1 | Board-quality bar tuned on absolute judge scores | Accept | Same-brief A/B against best-so-far and a frozen v1 baseline drives tuning; 1 to 5 scores logged only; Board Bar gated on outside raters; Buildable became a validator badge; Read and Story judged on the hero with text masked; calibration boards made from tuning briefs at v1 with an outside rater (ART_BOARDS 6, ENGINEERING 10, KICKOFF 5 and 6) |
| 2 | Realism bar too small, not held out, not blind | Accept | Whole builds held out (3 builds, 30 pairs, second light setup), intervals reported with a lower-bound gate, RAW plus a fixed processing step, halo and noise check on renders, camera match from the baseplate stud grid with a pixel threshold, Chris's own-photo votes excluded, open realism test added, Gemini counts on all still dimensions, twin pairs score only dimensions that can differ (GOLD_SET, ENGINEERING 10, KICKOFF 6) |
| 3 | Tune loop can't fit its limits | Accept | Split into a look loop on frozen models and a slower design loop at preview quality; keep-best rule written for a vector; round counts and time stops come from the Phase 2 cost model (KICKOFF 2 and 6, ENGINEERING 7) |

## Major

| # | Finding | Decision | What changed |
|---|---|---|---|
| 4 | `lego-motion` repo name | Accept in part | Chris chose to keep the name knowing the policy (asked Oct 17). The fan scan allowlists git metadata only; plugin, skills and outputs use brick names; README says so (FAN_RULES 2, ENGINEERING 1, README) |
| 5 | Art Director approves its own boards | Accept | The Creative Director approves boards; Chris signs (STUDIO_DELTA 3 to 5, ART_BOARDS 4, ENGINEERING 4) |
| 6 | Untouched clay rules wrong for bricks | Accept | Phase 2 writes one merged `STUDIO_BIBLE_BRICK.md`; STUDIO_DELTA 9 lists the clay lines it must rewrite; build gates regain a Concurs column |
| 7 | Shared core breaks once installed | Accept | Decided in Phase 2: separate `studio-core` package, generic job model, own `brick` entry point, shared skills copied with a version line, pointer in the clay kit only with Chris's OK (ENGINEERING 2, CLI renamed throughout) |
| 8 | Validator grid rejects legal builds; motion frames can't be grounded | Accept | Connection became the test (L3); grid alignment a warning; validator profiles `static`, `posed`, `fx`; a known-good suite added (BUILD 4, MOTION 6) |
| 9 | Locks miss Bash-based edits | Accept | Authoring commands and generators refuse approved models; Bash lock hook; hash check in `validate`; catalog built in the data folder (BUILD 9, ENGINEERING 5) |
| 10 | No machine-readable build bible | Accept | `build_bible.yaml` per job, `build_brief.md` rendered from it (DESIGN_LANGUAGE 9, CLAUDE_MAPPING folders) |
| 11 | Official printed parts allowed | Accept | All patterned parts banned by default; Librarian allowlist with reasons; prints must point to our `prints/` (BUILD 2 and 6, FAN_RULES 3) |
| 12 | No check for copies of official sets | Accept | L9 inventory overlap against official set inventories, routed to the Creative Director; Official Model Repository files test-only (BUILD 3 and 4) |
| 13 | Set board reads as a box front; no mark limits | Accept | House mark bottom right everywhere; piece count moved to the data panel; written mark and glyph brief (FAN_RULES 2, ART_BOARDS 2 and 5, LOOK_DEV 3) |
| 14 | Fan scan undefined; names leak into agent text | Accept | Three-layer scan with planted faults; terms list is data only the scan reads; charter paraphrases the references; disclaimer only in NOTICE and README (ENGINEERING 5, CLAUDE_MAPPING skills) |
| 15 | "Never sent to an API" vs agents viewing images | Accept | Inspiration material human-only by default; agents view a folder only with Chris's approval; study images from Chris's own boxes and booklets, nothing from the toy maker's site (FAN_RULES 5, GOLD_SET 3, KICKOFF 1) |
| 16 | Read test has no pass rule | Accept | Must-name nouns with synonyms approved at Build Brief, non-Claude reader, code match, calibrated on known-bad boards, unlabeled render (DESIGN_LANGUAGE 2) |
| 17 | Gold shoot can't make exact twins | Accept | Chris shares his parts first; builds use only confirmed parts; swaps logged; lock rules added (GOLD_SET 1) |
| 18 | Tuning crew writes held-out briefs | Accept | Chris writes them, or a one-time agent with no memory; the Creative Director never reads them (GOLD_SET 5, KICKOFF 2) |
| 19 | Motion cards promised before motion exists | Accept | Turntable and builds-itself cards built in the board lane on stepped keys; acting waits for Phase 8 (ART_BOARDS 2, MOTION 5, KICKOFF 4) |
| 20 | License story misses the Blender side and the catalog | Accept | Option A (GPL-3.0-or-later) is the default; Option B states the bpy-side code is GPL; catalog never committed; Rebrickable credit in NOTICE (ENGINEERING 6, BUILD 10, NOTICE) |

## Minor

| # | Finding | Decision | What changed |
|---|---|---|---|
| 21 | Wrong build bible section | Accept | Now section 9 everywhere |
| 22 | Who signs the Build Brief | Accept | Standing waivers recorded with `decide`, per board type (STUDIO_DELTA 5, ART_BOARDS 4, KICKOFF 0) |
| 23 | bpy size and shared environment | Accept | Uses research/04's 880 MB; one shared environment path in `config.toml`. The clay kit's 1.2 GB figure is left for the Phase 2 pointer change, with Chris's OK |
| 24 | Two pixel filter ranges | Accept | 1.8 to 2.0 px |
| 25 | Overstated claims, dropped [U] tags | Accept | All nine fixed (BUILD, LOOK_DEV, FAN_RULES, ART_BOARDS, STORY_AND_SOUND, ENGINEERING, MOTION, GOLD_SET) |
| 26 | Rig has no waist | Accept | Waist yaw in MOTION, BUILD and the shoot's joint lineup |
| 27 | Skin tones | Accept | Reworded as part color; Phase 1 finds the missing codes |
| 28 | Skill names too close | Accept | Inherited `boards` renamed `storyboards` |
| 29 | SessionStart does too much | Accept | Light check at session start; full test in Phase 0 and CI |
| 30 | EEVEE and Workbench without a GPU | Accept | Workbench tested headless in Phase 0; Cycles fallback for thumbnails and the instruction look |
| 31 | Importer defaults vs the look spec | Accept | Importer settings pinned; house shaders replace imported materials; smoke-test bounding box measured with gaps off |
| 32 | Template path | Accept | `../clay-motion/templates/agent-template.md` |
| 33 | Hidden judge rule | Accept | "No more than 0.05 below the panel", as in clay |
| 34 | Where board jobs live | Accept | `~/.config/brick-studio/config.toml` |
| 35 | Gaps against the ask | Accept | Scenes defined (ART_BOARDS 3); backdrop modules (BUILD 3); the design-language sheet is the first board (ART_BOARDS 2) |
| 36 | Sharing honestly | Accept | "Made with an AI studio" on anything posted publicly; kids' names kept out (FAN_RULES 3 and 4) |
| 37 | Writing offenders | Accept | All rewrites applied; long sentences split |
| 38 | Writing rule for agents | Accept | Kickoff writing rule covers en dashes, slogans and the flagged patterns, for STATUS.md and gate summaries too |
