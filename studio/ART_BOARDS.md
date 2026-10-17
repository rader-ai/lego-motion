# Art boards: the fast lane

Owner: the **Art Director**; the **Creative Director** approves; Chris signs. Chris chose art boards first (Oct 17, 2026): any idea becomes a finished brick-built board in hours, and every model built for a board carries straight into motion later. Sources: research/03 sections 5 and 6 (box art and instruction conventions), research/08 (brickification), research/05 (look), research/06 sections 5 and 7 (where generated images fit).

## 1. What an art board is here

One composed page that shows an idea in brick design language well enough that a builder, a storyboard artist or an animator would know what to make. Every image on a finished board is a render of a real, validated LDraw model. Generated images can guide direction (step 3 below) but never appear on a finished board: image models get studs, part thickness and connections wrong, and no tool has been shown to keep a stud grid intact through a restyle (research/06 sections 2 and 3).

## 2. Board types

| Type | What's on it | Look | Light preset |
|---|---|---|---|
| **Idea board** (default) | Hero render at a three-quarter high angle, 2 to 3 detail crops, the one-line read, palette chips, scale, part count | Production's look | `studio_box` or `practical` |
| **Set board** | Hero filling most of the frame, figures in action in front, a brick-built backdrop with gentle depth of field, 1 to 3 round insets of play features, a figure lineup; piece count lives in the data panel, not on the face | Set look | `studio_box` |
| **Instruction sheet** | 4 to 8 build steps: step numbers, a parts box per step with quantities, one sub-assembly panel, a rotate icon, a final figure lineup; almost no words | Flat with outlines | `instructions` |
| **Style frame** | One cinematic frame of a moment at film look | Film look | `practical` or `daylight` |
| **Character sheet** | Figure front, side, back; 4 to 6 face prints; accessories; scale bar in studs | Production's look | `studio_box` |
| **Beat board** | 4 to 6 frames telling one moment, with timing notes | Film look | per scene |
| **Motion card** | A 2 to 5 second loop. In the board lane: a **turntable** or a **builds-itself** from `steps.json`, both on stepped keys (MOTION.md section 5). Figure acting, faces and Brick FX come with the motion lane. | Production's look | per scene |

- **The first board** the studio makes is a design-language sheet: one model shown in set look and film look, the house palette chips, the scale ladder, and good and bad examples from the failure checklist (DESIGN_LANGUAGE.md section 10). It's the house style on one page.
- Set boards and instruction sheets follow the general conventions of real box art and building instructions (research/03 sections 5 and 6), in **our own layouts, type and house mark**. Never the toy maker's logo, box design, color bands or booklet layout, and the overall page must not read as a box front (FAN_RULES.md).

## 3. Scenes

A **scene** is a reusable unit: a validated set model (with its backdrop module) plus saved camera and light presets. Any board, motion card or film shot can reuse it, so "render this idea as a scene" means build or pick the set, pick a camera and light preset, and render. Scenes live in `scenes/<name>/` with `scene.json` (model, backdrop, cameras, lights, look) and are approved like models (BUILD.md section 9).

## 4. The lane, step by step

| # | Step | Who | Output | Check |
|---|---|---|---|---|
| 1 | **Brief.** Chris's idea in a sentence, plus the board type (default Idea board) | Chris | `brief.md` | n/a |
| 2 | **Brickify.** The 10-step method (DESIGN_LANGUAGE.md section 2) | Art Director | `build_bible.yaml`, rendered as `build_brief.md` | Chris taps approve, or Chris's recorded standing waiver for that board type applies (`brick decide --type waiver`) |
| 3 | **Concept sketch** (optional, open-ended briefs only). 4 to 12 generated images to explore silhouette and palette, labeled as sketches | Visual Development Artist | `concept/` | Never used as a frame; fan-scanned |
| 4 | **Build.** Model and backdrop written as LDraw from code | Model Builder (Build Designer, Figure Designer specs) | `model.mpd`, `bom.csv` | n/a |
| 5 | **Validate.** Static profile (BUILD.md section 4) | Build Validator (code) | `validate.json` | Hard gate; fails go back to step 4 |
| 6 | **Read test.** On the unlabeled hero render (DESIGN_LANGUAGE.md section 2) | Technical QC | `read_test.json` | Must name one of the `must_name` nouns; fail goes back to step 2 or 4 |
| 7 | **Look and camera.** Wear level, lens, light preset, or a saved scene | Look Dev Artist, Camera and Lighting Operator | `shot.json` per image | Screen-test baselines |
| 8 | **Render.** Hero and details in Cycles; instruction steps in the instruction tier | Pipeline (`brick board render`) | `renders/` | Look QC |
| 9 | **Compose.** HTML board template filled with renders and data, exported to PNG and PDF | Board Designer | `board.png`, `board.pdf` | Fan scan, all three layers |
| 10 | **Review.** Same-brief comparison and notes from the critic panel, then the Creative Director approves, then Chris signs | Critic Panel, Creative Director | `critics.json`, notes | Board Final lock |

The Art Director owns the board and never approves it. Board jobs go in the boards repo named in `config.toml` (ENGINEERING.md section 1).

## 5. Layout system (Board Designer)

- **Two formats:** landscape 1920 x 1080 for sharing and a big screen; portrait 1080 x 1350 for the phone. Every template exists in both.
- **Templates are HTML and CSS** filled from a JSON sidecar (renders, title, read line, palette codes, part count, scale) and printed to PNG and PDF with headless Chromium. Type stays crisp and layouts stay editable in code.
- **Type:** one house typeface pair with an open license, chosen in Phase 0 and recorded in the license ledger. Never the toy maker's fonts.
- **Grid:** a 12-column grid with generous margins; the hero takes at least half the board.
- **Palette chips:** small rendered 1x2 bricks in each production color with the LDraw code, not flat swatches.
- **Callouts:** round magnifier insets with a thin rule, used for play features and details.
- **Data panel:** scale, part count, unique parts, palette codes, validator badge.
- **Instruction pages:** white page, large step number, parts box per step ("2x" quantities, parts drawn at consistent scale), one pale house-tint panel for sub-assemblies, rotate icon as a circular arrow around a tiny model, no sentences.
- **House mark:** small, **bottom right on every board**, following the mark brief in FAN_RULES.md section 2. Chris picks it in Phase 0.
- **Footer:** the LDraw credit line and, on anything shared publicly, "Made with an AI studio" (FAN_RULES.md section 4).

## 6. What good looks like

The rubric lines, used by people and by the critic panel:
1. **Read:** names the subject in one second at phone size.
2. **Design language:** palette, scale and part vocabulary follow the build bible; feels like a real set or the films without copying either.
3. **Plastic:** looks like a macro photo (LOOK_DEV.md tells checklist).
4. **Story:** a moment, a motive, one gag.
5. **Layout:** clean hierarchy, hero dominant, text minimal.

**Buildable** isn't a rubric line. It shows as the validator badge, decided only by code.

How it's measured (ENGINEERING.md section 10):
- **During tuning:** the critic panel compares a candidate board against the best board so far for the **same brief**, and against a frozen v1 baseline, order-flipped, line by line. Read and Story are judged on the hero render with all board text masked.
- **1 to 5 scores** are logged for diagnosis only, never optimized (clay research/04).
- **At the Board Bar:** outside raters score the held-out boards (section "The Board Bar" in KICKOFF Phase 6).

## 7. Time and cost targets

- Target: a first Idea board in under 2 hours wall clock from brief, with Chris spending under 5 minutes (brief plus two taps).
- Render time per board is measured in Phase 0 on the render machine and drives these targets.
- Concept sketches cost a few cents each on current image models (research/06 section 5); building and rendering are local compute.

## 8. Where boards go next

An approved board's models, figures, scenes and build bible become a production's starting assets: style frames and beat boards feed the animatic, character sheets feed figure builds, instruction sheets feed builds-itself shots, and scenes become sets. Nothing made for a board is thrown away.
