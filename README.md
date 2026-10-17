# Brick Studio

A starter kit for an AI brick studio, built as a Claude Code plugin named `brick-studio`. It turns any idea into brick-built art boards first, then scenes and short films. Models are built from real brick parts in code and rendered in Blender, with Claude agents as the crew, aiming at the craft of the best brick-built features and brick stop-motion films.

Sister project: [rader-ai/clay-motion](https://github.com/rader-ai/clay-motion). Brick Studio shares its operating model (roles, gates, tracker, critic panel) and lists only what's different.

**Status: v0.2 (design).** No code yet. This repo holds the design docs, the research, and the prompt that has Claude Code build the rest. One independent review has been applied (see `reviews/`).

The repo name is Chris's choice; the plugin, skills and every output use brick names only (`studio/FAN_RULES.md` section 2).

**Fan project.** Personal and non-commercial. Not sponsored, authorized or endorsed by the LEGO Group. Brick parts come from the LDraw Parts Library (ldraw.org), CC BY 2.0. See `NOTICE` and `studio/FAN_RULES.md`.

## Start here

1. Read `KICKOFF_PROMPT.md` (setup steps for Chris at the top).
2. Open Claude Code on Opus 5.5 in this repo, with `clay-motion` cloned next to it, and paste the prompt.

## What's here

| Path | What it is |
|---|---|
| `KICKOFF_PROMPT.md` | The prompt: setup, research refresh, spec, review, build the board lane, calibrate, run the look and design tune loops to the Board Bar, pilot boards, then the motion lane |
| `studio/CREATIVE_DIRECTION.md` | Chris's direction and decisions, and the open choice of thread |
| `studio/STUDIO_DELTA.md` | How the org differs from the clay studio's bible: new roles, board crew, gates, the bar, and the clay rules a merged brick bible must rewrite |
| `studio/DESIGN_LANGUAGE.md` | The brick design language: brickification method, scale, palette, parts, two looks, story, abstract ideas, failure checklist |
| `studio/ART_BOARDS.md` | The fast lane: board types, scenes, steps, layout system, how boards are judged |
| `studio/BUILD.md` | Building, validating and cataloging brick models in code |
| `studio/LOOK_DEV.md` | Plastic shader, geometry, wear, lens, light, color, post, screen test |
| `studio/MOTION.md` | How bricks and figures move: style guide, engine changes, brick effects, builds-itself, motion QC |
| `studio/STORY_AND_SOUND.md` | Story and sound notes for brick worlds |
| `studio/CLAUDE_MAPPING.md` | Models per role, skills, workflows, folders |
| `studio/ENGINEERING.md` | Repos, Blender and LDraw pinning, shared core, validator, CLI additions, compute, tests |
| `studio/GOLD_SET.md` | What the judges compare against, and Chris's brick shoot |
| `studio/FAN_RULES.md` | What we can use and what stays off limits |
| `research/01-09` | Nine research reports from Oct 17, 2026, with sources |
| `reviews/` | Independent review of this kit and how each finding was resolved |
| `NOTICE` | Credits and the fan-project disclaimer |

Research was gathered by Sonnet research agents on Oct 17, 2026. A shared search budget ran out partway through several tracks, so each report lists its gaps; versions, prices and licenses move monthly, and the prompt re-verifies them first.
