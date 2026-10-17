# Art boards: the fast lane

Owner: the **Art Director**. Chris chose art boards first (Oct 17, 2026): any idea becomes a finished brick-built board in hours, and every model built for a board carries straight into motion later. Sources: research/03 sections 5 and 6 (box art and instruction conventions), research/08 (brickification), research/05 (look), research/06 sections 5 and 7 (where generated images fit).

## 1. What an art board is here

One composed page that shows an idea in brick design language well enough that a builder, a storyboard artist or an animator would know what to make. Every image on a finished board is a render of a real, validated LDraw model. Generated images can guide direction (step 3 below) but never appear on a finished board, because image models get studs, part thickness and connections wrong and no tool reliably keeps a stud grid through a restyle (research/06 sections 2 and 3).

## 2. Board types

| Type | What's on it | Look | Light preset |
|---|---|---|---|
| **Idea board** (default) | Hero render at a three-quarter high angle, 2 to 3 detail crops, the one-line read, palette chips, scale, part count | Production's look | `studio_box` or `practical` |
| **Set board** | Hero filling 60 to 80% of the frame, figures in action in front, a themed backdrop with gentle depth of field, 1 to 3 round insets of play features, a figure lineup, piece count, house mark | Set look | `studio_box` |
| **Instruction sheet** | 4 to 8 build steps: step numbers, a parts box per step with quantities, one sub-assembly panel, a rotate icon, a final figure lineup; almost no words | Flat with outlines | `instructions` |
| **Style frame** | One cinematic frame of a moment at film look | Film look | `practical` or `daylight` |
| **Character sheet** | Figure front, side, back; 4 to 6 face prints; accessories; scale bar in studs | Production's look | `studio_box` |
| **Beat board** | 4 to 6 frames telling one moment, with timing notes | Film look | per scene |
| **Motion card** | A 2 to 5 second loop: turntable, a builds-itself, or one acting beat | Production's look | per scene |

Set boards and instruction sheets follow the general conventions of real box art and building instructions (research/03 sections 5 and 6) in **our own layouts, type and house mark**. Never the toy maker's logo, box design, color bands or booklet layout (FAN_RULES.md).

## 3. The lane, step by step

| # | Step | Who | Output | Check |
|---|---|---|---|---|
| 1 | **Brief.** Chris's idea in a sentence, plus the board type (default Idea board) | Chris | `brief.md` | n/a |
| 2 | **Brickify.** The 10-step method (DESIGN_LANGUAGE.md section 2) | Art Director | `build_brief.md` | Chris taps approve, or the job proceeds on the Art Director's call if Chris said "just make it" |
| 3 | **Concept sketch** (optional, open-ended briefs only). 4 to 12 generated images to explore silhouette and palette, labeled as sketches | Visual Development Artist | `concept/` | Never used as a frame |
| 4 | **Build.** Model written as LDraw from code | Model Builder (Build Designer, Figure Designer specs) | `model.mpd`, `bom.csv` | n/a |
| 5 | **Validate.** Parts and colors exist, grid, collisions, connected and stable, budget | Build Validator (code) | `validate.json` | Hard gate; fails go back to step 4 |
| 6 | **Read test.** Thumbnail plus black-fill silhouette shown to a vision model | Technical QC | `read_test.json` | Names the subject; fail goes back to step 2 or 4 |
| 7 | **Look and camera.** Wear level, lens, light preset | Look Dev Artist, Camera and Lighting Operator | `shot.json` for each image | Screen-test baselines |
| 8 | **Render.** Hero and details in Cycles; instruction steps in the instruction tier | Pipeline (`studio render`) | `renders/` | Look QC (stud mark, no logo, no blur gradient) |
| 9 | **Compose.** HTML board template filled with renders and data, exported to PNG and PDF | Board Designer | `board.png`, `board.pdf` | Fan-rules scan (logos, banned names) |
| 10 | **Review.** Critic panel on the rubric, then the Art Director, then Chris | Critic Panel, Art Director | `critics.json`, notes | Board Final lock |

The builds-itself motion card reuses the same `model.mpd` plus the Instructions Designer's `steps.json` (MOTION.md section 5).

## 4. Layout system (Board Designer)

- **Two formats:** landscape 1920 x 1080 for sharing and a big screen; portrait 1080 x 1350 for the phone. Every template exists in both.
- **Templates are HTML and CSS** filled from a JSON sidecar (renders, title, read line, palette codes, part count, scale) and printed to PNG and PDF with headless Chromium. This keeps type crisp and layouts editable in code.
- **Type:** one house typeface pair with an open license, chosen in Phase 0 and recorded in the license ledger. Never the toy maker's fonts.
- **Grid:** a 12-column grid with generous margins; the hero takes at least half the board.
- **Palette chips:** small rendered 1x2 bricks in each production color with the LDraw code, not flat swatches.
- **Callouts:** round magnifier insets with a thin rule, used for play features and details.
- **Instruction pages:** white page, large step number, parts box per step ("2x" quantities, parts drawn at consistent scale), one pale house-tint panel for sub-assemblies, rotate icon as a circular arrow around a tiny model, no sentences.
- **House mark:** small, top left on Set boards, bottom right on others. Chris approves the mark in Phase 0.
- **Footer credit:** the LDraw credit line (FAN_RULES.md section 4).

## 5. What good looks like (the board rubric)

Scored by the critic panel (pairwise against gold photos or prior approved boards where that fits) and checked by the Art Director:
1. **Read:** names the subject in one second at phone size.
2. **Design language:** palette, scale and part vocabulary follow the build bible; feels like a real set or the films without copying either.
3. **Plastic:** looks photographed (LOOK_DEV.md tells checklist).
4. **Story:** a moment, a motive, one gag.
5. **Layout:** clean hierarchy, hero dominant, text minimal.
6. **Buildable:** passed the validator (code only; never judged by a model).

## 6. Time and cost targets

- Target: a first Idea board in under 2 hours wall clock from brief, with Chris spending under 5 minutes (brief plus two taps).
- Render time per board is measured in Phase 0 on the render machine and drives these targets.
- Concept sketches cost a few cents each on current image models (research/06 section 5); building and rendering are local compute.

## 7. Where boards go next

An approved board's models, figures and build bible become a production's starting assets: Style frames and Beat boards feed the Animatic; Character sheets feed figure builds; Instruction sheets feed builds-itself shots. Nothing made for a board is thrown away.
