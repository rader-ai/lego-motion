# How Brick Studio runs on Claude

Companion to `STUDIO_DELTA.md`. The clay studio's `CLAUDE_MAPPING.md` applies in full (the five rules, model and effort by seniority, which Claude Code feature does which job, handoff packets, what stays human). This file lists the brick roles, skills, workflows and folders. Model names and Claude Code features were checked for the clay kit on Oct 17, 2026; re-verify in Phase 0.

## The short version

- Chris is the Executive Producer and signs every gate and lock. For boards, that's two taps: the build brief and the finished board, after the Creative Director approves it.
- The main Claude Code session on Opus 5.5 is the Producer.
- Makers on Sonnet 5.5, heads and reviewers on Opus 5.5, clerical work on Haiku 5.5, anything measurable runs as code.
- The equipment: Blender 5.2 LTS from Python, the LDraw parts library, ffmpeg, headless Chromium for board layout, and Rhubarb for lip sync. Image models are allowed only for concept sketches, labeled and never in a final frame. The only outside APIs by default are the critic panel's Gemini and OpenAI judges and, optionally, one image model for sketches.

## Models and effort for brick roles

Roles unchanged from clay keep the clay table. New and renamed roles:

| Role | Level | Model | Effort |
|---|---|---|---|
| Art Director | Head | Opus 5.5 | high |
| Board Designer | Artist | Sonnet 5.5 | medium |
| Figure Designer | Artist | Sonnet 5.5 | high |
| Build Designer | Artist (exception) | Opus 5.5 | high |
| Head of Build | Head | Opus 5.5 | high |
| Model Builder | Artist | Sonnet 5.5 | high |
| Print and Decal Artist | Artist | Sonnet 5.5 | medium |
| Instructions Designer | Artist | Sonnet 5.5 | medium |
| Brick Librarian | Senior | Sonnet 5.5 | medium |
| Brick FX Artist | Artist | Sonnet 5.5 | medium |
| Build Validator | Gate | Code | n/a |

**Exceptions and why**
- **Build Designer on Opus:** turning an idea into a buildable model is the hardest reasoning step in the board lane. A recent benchmark found frontier models with a part-placement toolchain and a validator in the loop produce valid builds almost every time, while design quality still trails human builders (research/06 section 4). Better design judgment is worth the spend.
- **Model Builder on Sonnet, high effort:** it writes LDraw against a clear spec with the validator answering every step, so the loop catches mistakes; effort buys fewer loops.
- **Brick Librarian on Sonnet:** most of the job is data (part and color tables, pivots); judgment calls go to the Head of Build.

## Skills

| Skill | Used by | What it carries |
|---|---|---|
| `studio-charter` | Everyone | The merged brick bible (STUDIO_DELTA.md section 9), FAN_RULES.md and the other studio docs in `reference/`. Its copy of CREATIVE_DIRECTION.md describes the references as "the brick-built feature films and official sets" instead of quoting names. No disclaimer text; that lives in NOTICE. |
| `board` | Producer | Entry point: `/brick-studio:board <idea> [--type idea\|set\|instructions\|style\|character\|beat\|motion]` runs the board lane in the boards repo from `config.toml` |
| `make` | Producer | Entry point for films: `/brick-studio:make <brief>` runs a production through every lock |
| `brickify` | Art Director, Build Designer, Visual Development Artist | The 10-step method, scale table, palette rules, abstract-idea grammar, failure checklist |
| `build-bible` | Art Director | Build bible template, look modes, fan-rules check |
| `brick-build` | Build Designer, Model Builder, Figure Designer | LDraw authoring helpers, part search, connection placement, modules, figures, BOM (BUILD.md) |
| `brick-library` | Brick Librarian | Catalog build, part-color availability, pivots, LDraw updates, stud mark setting |
| `validate` | Technical QC, Model Builder | Validator profiles, reading `validate.json`, common fixes |
| `instructions` | Instructions Designer, Board Designer | Step ordering, sub-assemblies, instruction render tier, page templates |
| `prints-and-decals` | Print and Decal Artist, Figure Designer | Face sets, torso and tile prints, decal UV conventions, house print style |
| `plastic-look` | Look Dev Supervisor and Artist | Shaders, wear stack, jitter, stud glyph, screen test (LOOK_DEV.md) |
| `camera-and-light` | DP, Operator | Lens kit math, toy-eye heights, light presets |
| `board-layout` | Board Designer | HTML templates, JSON sidecar schema, export, house type |
| `animate-shot` and `animation-engine` | Animators, Animation TD | Shared engine plus MOTION.md changes (rigid figure, stud solver, face slots) |
| `brick-fx` | Brick FX Artist, VFX Supervisor | FX voxelizer, FX palettes, destruction from connections |
| `critic-panel`, `continuity-check`, `dailies`, `sound`, `story-development`, `storyboards`, `animatic-and-edit` | As in clay | Copied from the clay design into this plugin with a version line at the top (plugins can't share skills), plus brick notes (STORY_AND_SOUND.md). Clay's `boards` skill is renamed `storyboards` here so it can't be confused with `board`. |

## Workflows

| Workflow | Does |
|---|---|
| `research-refresh` | As in clay, over research/01 to 09 |
| `board-batch` | Runs the board lane for several briefs in parallel, up to the render budget |
| `screen-test` | Renders material and wear changes on the screen-test scene and scores them against Chris's photos |
| `library-update` | Pulls a new LDraw release, rebuilds the catalog, re-validates every approved model, reports breaks |
| `shoot-sequence`, `run-dailies`, `tune` | As in clay, with brick QC |

## Folders

**Board job** (lives inside a boards repo, one folder per board):
```
boards/<yyyy-mm-dd>-<slug>/
  brief.md  build_bible.yaml  build_brief.md  STATUS.md
  concept/              optional labeled sketches
  model/                model.mpd, bom.csv, steps.json, validate.json
  figures/<name>/       figure.ldr, face set decals
  shots/                shot.json per image
  renders/              gitignored
  board/                board.json, board.png, board.pdf
  review/               read_test.json, critics.json, notes.md
```

**Scenes** live in `scenes/<name>/` (`scene.json`, model, backdrop, camera and light presets) in the boards repo and are reused across boards and films (ART_BOARDS.md section 3).

**Film production:** the clay production folder, with `bible/build_bible.yaml` in place of the material bible, `models/`, `figures/` and `scenes/` in place of `characters/` and `sets/`, and `validate.json` beside every model.

## What stays human

As in clay, plus: Chris approves the house mark, the stud glyph and the house typeface; Chris writes the held-out briefs; Chris's brick shoot (GOLD_SET.md); viewing the private inspiration material (human-only by default, FAN_RULES.md section 5); and the final call on whether an idea is fine to make under fan rules when the Creative Director flags it.
