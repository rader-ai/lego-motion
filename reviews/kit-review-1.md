# Review of Brick Studio kit v0.1 (independent, Oct 17, 2026)

I read the README, the kickoff prompt, all 12 studio docs and the nine research files. I also read the clay kit files the brick kit leans on: STUDIO_BIBLE, CLAUDE_MAPPING, ENGINEERING, MOTION, GOLD_SET, STORY, KICKOFF_PROMPT, research/04 and both clay reviews. I changed no file except this one.

Two notes before the findings:
- A commit landed while I was reading (2c967c4, which renames the repo to `lego-motion`). This review covers the kit after that commit.
- My fetch of the Claude Code docs timed out. Where a Claude Code point rests on the clay kit's own notes, I mark it [verify].

The short version: the design language, look dev and build docs are strong and mostly true to the research. I spot-checked 30 numbers and claims (table at the end). 21 match; the rest overstate the research or drop its [U] tag. There are no em or en dashes anywhere in the kit. The weak spots are the Board Bar (how "good" is measured and tuned), places where "only the deltas live here" leaves clay rules that are wrong for bricks, and fan-rule gaps around official prints, set copies and the new repo name.

To keep this file inside the kit's own fan rules, I say "the toy maker" and "the first film" instead of real names.

**Counts:** 3 Blocker, 17 Major, 18 Minor.

---

## Blocker

### 1. The board-quality half of the Board Bar isn't a safe target

**Where:** KICKOFF Phase 6 /goal ("board rubric averages at least 4.0 of 5"); ART_BOARDS section 5; ENGINEERING section 10; KICKOFF Phase 5.4.

**Problem:**
- The tune loop optimizes absolute 1 to 5 scores from model judges. The research this kit inherits says not to. Clay research/04 says anchored 1 to 5 scores are "logged for diagnosis, never optimized". It also says Claude scores about a point stricter than Gemini, so absolute scales can't be mixed or averaged.
- Rubric line 6, "Buildable", is code only, but the /goal averages "the board rubric" with "no dimension under 3.5". It's unclear whether a judged score for buildability sneaks into the average.
- Judges see the whole board, including its title and the one-line read. A good caption can lift the "Read" and "Story" scores while the picture stays the same.
- ART_BOARDS says boards are scored "pairwise against gold photos or prior approved boards". A new idea has no gold photo, and no board is approved until Phase 7.
- Phase 5.4 wants "75% judge agreement with the human majority", but only Chris rates boards. It also doesn't say where the 20 rated boards come from.
- Board Bar has no outside-rater threshold for held-out boards (Phase 6.7 names the raters but no number).

**Fix:**
- For keep-best, use pairwise A/B on the same brief: the candidate against best-so-far and against a frozen v1 baseline, order-flipped, per rubric line. That answers "did this change make boards better" without needing any gold.
- Log the 1 to 5 scores for diagnosis only. Gate Board Bar on people: outside raters score the held-out boards against a set threshold (for example, a mean of 4.0 and at least 70% of boards rated 4 or 5).
- Take "Buildable" out of the scored rubric. Show it as a validator badge.
- Judges score Read and Story from the hero render with all board text masked.
- Phase 5 makes the 20 calibration boards from the tuning briefs at v1, and adds at least one outside rater so "majority" means something.

### 2. The realism half of the Board Bar is too small, not truly held out, and not blind

**Where:** GOLD_SET "Sizes and splits" ("10 gold held out"); GOLD_SET "Why this matters" ("Differences can only come from the look"); KICKOFF Phase 6 /goal; ENGINEERING section 10 ("Panel as in clay").

**Problem:**
- There are few twin pairs. 6 to 10 builds, each shot from one set angle plus one free angle, gives 12 to 20 twin photos. Holding out 10 leaves 2 to 10 for tuning. With 10 held-out pairs, a 0.40 win rate has a 95% interval of about plus or minus 0.30. The /goal can pass or fail on noise.
- The held-out photos show the same builds, bricks, desk and lamp as the tuning photos. The loop can fit that one setup and still pass.
- Chris shot every twin photo, so his checkpoint A/B can never be blind. The clay GOLD_SET, which this kit says it follows, doesn't count his votes on his own footage toward the bar. Here every realism pair is his own footage, and his A/B still drives a /goal stop.
- Phone photos carry phone processing: sharpening, noise reduction, HDR. A judge can tell a render from a phone photo by that alone. The cheapest way for the loop to raise its win rate is to fake that processing. LOOK_DEV section 8 bans sharpening halos, but no code checks it.
- No step matches the twin's camera and light (focal length, sensor crop, camera pose). Without it, "same angle, matched light" won't hold. Differences will also come from match error, part mold versions and real wear, not only "the look".
- Two of the six still dimensions, "design language and read" and "scale and lens", are the same on both sides of a twin pair by design, so they measure nothing there.
- Twins are lit by one desk lamp. Boards use studio_box, practical and daylight. A pass on twins says little about boards.
- The clay panel counts Gemini only on pose and performance, because Gemini samples video at about 1 frame per second. That reason doesn't apply to stills, but the brick kit never says so. "2 of the 3 judge families" is unclear as a result.

**Fix:**
- Hold out whole builds, not photos: at least 3 builds, shot under a second light setup the loop never sees. Aim for at least 30 held-out pairs, report intervals, and gate on the lower bound.
- Shoot RAW (or the phone's least-processed mode). Run every gold photo through one fixed, written-down processing step. Add a code check for sharpening halos and noise level on renders.
- Add a camera-match step: EXIF focal length, then a pose solve from the baseplate's stud grid, which is a free calibration target. Give it an owner (the DP) and a pass threshold in pixels of reprojection error.
- Leave Chris's votes on his own photos out, as clay does. Use outside raters for the stop guard.
- Add a non-twin realism check: board hero renders paired with random gold photos, clay-style, so the look has to hold outside the twin setup.
- State that Gemini counts on every still dimension. In twin pairs, score only the dimensions that can differ.

### 3. The tune loop can't run inside its own limits

**Where:** KICKOFF Phase 6.1 and 6.2 ("best mean panel result on the tuning briefs over 3 seeds"); the /goal ("25 rounds", "36 hours"); ART_BOARDS section 6.

**Problem:** Phase 6.1 lets a round change brickify rules and skill text. So every candidate has to rerun the full board lane: 10 tuning briefs times 3 seeds is 30 boards per candidate per round. ART_BOARDS targets 2 hours per board. research/06 section 4 reports $0.87 to $6.33 per assembly for frontier models, with Opus 5.5 at $6.33, and the Build Designer runs on Opus. If a board costs about what one benchmark assembly costs, one candidate per round is about $190 in build calls alone, before renders and judges. 25 rounds is about $4,700. The 36-hour stop leaves under 90 minutes per round for 30 boards with final renders on one machine. The loop will hit the time stop long before the bar. "Best mean panel result" is also undefined, because the panel gives a realism vector and a rubric vector.

**Fix:**
- Split into two loops. A look loop changes only shaders, wear and light, and re-renders frozen, validated models (the twins plus 10 board models built once); seeds vary only jitter and wear. A design loop changes brickify rules and skills, runs the build stages at preview render quality, and runs fewer rounds.
- Write the keep-best rule for a vector: no dimension may drop by more than the seed spread, and the mean must rise by more than it.
- Phase 2 works out cost and time per round from the Phase 0 numbers, and sets the round count and time stop from that before the /goal is printed.

---

## Major

### 4. The repo name `lego-motion` breaks the rule it cites

**Where:** FAN_RULES section 2, row 1 ("One exception: Chris named the kit repo"); KICKOFF ground rules; ENGINEERING section 1.

**Problem:** The same table row gives the reason for the rule: the fan policy "bars the name in addresses" (research/07 section 4 says not in a domain name or internet address). A public GitHub URL is an internet address. The name will also sit in every install command and in the Phase 9 README, which travel with any board Chris shares. The fan scan checks file names, so it will flag the repo's own paths and git remote on every run and need a carve-out that weakens it. The local folder is still `brick-motion` and the README never names the repo, so the docs already disagree.

**Fix:** This is Chris's call, but now is the cheap time to change it. Rename the GitHub repo to `brick-motion` (GitHub redirects the old URL) or make it private. If it stays, add one allowlist entry for git metadata only, say so in the README, and keep the plugin, marketplace and every output name brick-only.

### 5. The Art Director approves its own boards

**Where:** STUDIO_DELTA section 3 ("Cannot approve its own boards; the Director approves") vs section 4 ("the Art Director approves and Chris signs"); ART_BOARDS step 10; ENGINEERING section 4 (`studio board final`).

**Problem:** The Art Director owns every board and is also its approver. That breaks clay rule 2 ("Nobody grades their own work") and contradicts the Art Director's own role entry.

**Fix:** Pick one approver who isn't the owner. The simplest is the Creative Director, which already owns the bar and the fan-rules check; then Chris signs. Update STUDIO_DELTA sections 3 and 4, ART_BOARDS step 10 and the `board final` refuse rule.

### 6. Clay roles that the delta doesn't touch are wrong for bricks

**Where:** STUDIO_DELTA intro ("RACI and escalation all apply here unchanged"); clay STUDIO_BIBLE 4.5, 4.8 and 9.1; clay MOTION section 6.

**Problem:**
- Clay's Visual Development Artist "Owns: art boards". Here the Art Director owns boards and the Board Designer lays them out.
- Clay's DP "Fails when: cinematic CG lighting, telephoto compression, ... sky lighting". The brick film look is "cinematic", the `daylight` preset is sun plus sky, and LOOK_DEV offers a 100 mm macro for box-art angles.
- The clay RACI has columns for the Production Designer and Fabrication and rows like "World and material bible". It has no row for the build brief, model validation, the catalog or a board.
- Clay motion QC warns on a "Near-straight spine on key poses". A rigid figure trips it on every pose.
- The brick build-gate table drops clay's "Concurs" column without a reason.
- Every agent has to read two bibles and work out which line wins, every time. That's where mistakes will come from.

**Fix:** In Phase 2, have the Producer write one merged `STUDIO_BIBLE_BRICK.md`: the clay bible with the delta applied, a brick RACI, rewritten DP and Visual Development entries, and the motion QC table without the spine check. Review it once at Spec Lock. Ship that one file in `studio-charter/reference/` instead of two.

### 7. The shared core plan breaks once the plugin is installed

**Where:** ENGINEERING section 2; KICKOFF Phase 0.2 ("If yes, depend on them"); CLAUDE_MAPPING skills ("Shared with clay").

**Problem:**
- `../clay-motion` exists only in Chris's working copy. An installed plugin can't count on a sibling folder.
- Both plugins would ship `bin/studio`. With both enabled, the one that runs is whichever comes first on PATH [verify].
- The clay kit says nothing about a shared core (I searched it). If Chris builds clay next, it will build its own `lib/` as its ENGINEERING section 2 says.
- Clay's tracker is built around shots: packets per shot, pop-through approval, Voice Lock. Board jobs have other locks and folders. The kit doesn't describe a job and lock model that fits both.
- Skills listed as "shared with clay" have no way to be shared. Each plugin needs its own copy.

**Fix:** Decide this in Phase 2, not Phase 0. Make `core` a separate Python package (for example `rader-ai/studio-core`) with a generic job, lock and packet model, pinned by version in each plugin's environment. Give the brick entry point its own name (for example `brick`) so two plugins never ship the same binary. With Chris's OK, add a one-line pointer in clay ENGINEERING section 2. Copy shared skills into each plugin with a version line at the top.

### 8. Validator L3 rejects legal builds

**Where:** BUILD section 4, L3 ("positions on 20 LDU (X, Z) and 8 LDU (Y)"); STUDIO_DELTA section 3 ("Every model and every frame of motion passes").

**Problem:** LDraw part origins don't all sit on one 20 LDU grid. Even-width and odd-width parts are offset by half a stud from each other, so a 1x2 brick seated on a 2x4 lands at 10 LDU in Z (check against the pinned library). Jumper plates offset by half a stud, and many sideways-stud and bracket builds land off the grid. The exceptions cover hinges, clips, bars and sideways studs only. research/04 section 4 tags this method "[U, my method]"; BUILD presents it as the rule. Separately, STUDIO_DELTA says every frame of motion must be "connected and stable", which fails every jump and every Brick FX frame by design.

**Fix:** Make connection the test: every part has at least one typed contact matched within 0.5 LDU (pyldraw3 contacts), and grid alignment becomes a warning. Prove it on known-good models (a 1x2 on a 2x4, a jumper offset, a sideways-stud wall) as well as the known-bad suite. Add validator profiles: `static` (L0 to L9), `posed` (L6 and L7 off for figures in the air), and `fx` (FX parts skip connection checks but still must be real parts in real colors).

### 9. Locks don't cover the main way models get written

**Where:** ENGINEERING section 5 ("Locked models | PreToolUse on Edit and Write"); ENGINEERING section 4 (`place` refuses only off-connection placements); ENGINEERING section 2 (`catalog/` inside the plugin).

**Problem:** Agents build models with `studio brick place`, `studio brick module` and Python generators, all through Bash. The lock hook watches only Edit and Write, and `place` has no "model is approved" refusal. So an approved model can change without reopening approval. The catalog is written into the plugin folder, which is the installed copy [verify whether updates replace it].

**Fix:** `place`, `module` and every generator refuse when the model's manifest is approved. Add a Bash PreToolUse hook, like clay's "Protect state from the shell", for any `.mpd` with an approved manifest. `validate` fails when a file's hash doesn't match its approved manifest. Build the catalog into the plugin data folder.

### 10. Board jobs have no build bible the validator can read

**Where:** BUILD section 4, L2 ("every color is in the build bible palette"); DESIGN_LANGUAGE section 9; CLAUDE_MAPPING board folder layout.

**Problem:** The validator needs the palette, the allowed and banned parts and the budgets as data. The build bible is `bible/build_bible.md`, which is prose, and the board folder has no `bible/` at all. L2 and L8 can't run, and rubric line 2 depends on the same file.

**Fix:** Every board job writes `build_bible.yaml` at Build Brief: look, scale, palette codes with their roles, seed part, allowed and banned parts, piece and print budgets. Render `build_brief.md` from it for Chris. Add it to the board folder layout and to the Phase 2 schema list.

### 11. Official printed parts are allowed by default

**Where:** BUILD section 2 ("Banned list: patterned parts whose name or keywords point to film or licensed characters"); FAN_RULES section 3 ("Every character, face print, torso print ... is ours").

**Problem:** Every LDraw patterned part copies the toy maker's print artwork: faces, torsos, printed tiles. research/07 section 3 says the toy maker's face and torso prints are its own artwork, so make original prints only. The banned list catches only licensed and logo prints. A generic official face or torso passes L1 and L9.

**Fix:** Ban every patterned part by default (LDraw pattern IDs and `!TEXMAP` parts). The Brick Librarian can allow a generic print, such as a plain clock face, with a written reason. Figures use prints from `prints/` only, and L9 checks that every printed figure part points to a `prints/` file with an "original" sidecar.

### 12. Nothing checks for a copy of an official set

**Where:** FAN_RULES section 3 ("No copies of official sets"); GOLD_SET section 3 (inspiration set as "a do-not-copy check").

**Problem:** The only check is the Creative Director looking at a private image folder. A model can match an official set part for part and pass every gate.

**Fix:** Add an L9 check that compares each model's parts list (part and color) with official set inventories from Rebrickable's set and inventory tables (research/09 section 6). Send any overlap above a threshold (for example 0.6 Jaccard) to the Creative Director. Never use LDraw Official Model Repository files as modules or starting points. Keep them in a test-only folder that the authoring tools can't read.

### 13. Set boards rebuild a box front, and the house mark has no limits

**Where:** ART_BOARDS section 4 ("House mark: small, top left on Set boards"); ART_BOARDS section 2, Set board row; FAN_RULES section 2 ("Box art without the logo needs something in that spot"); LOOK_DEV section 3 (stud glyph).

**Problem:** research/03 section 5 puts the box logo block "at top left" (tagged [U]). The Set board puts our mark in that spot, plus a hero filling 60 to 80% of the frame, round insets, a figure lineup and a piece count. Each piece is generic. Together they read as a box front, which the fan rules forbid. The kit also sets no limits on the mark or the stud glyph. research/07 section 8 lists what to avoid: a red oval, a red, white and black brand look, a lookalike wordmark. The glyph will be on every close-up stud.

**Fix:** Put the house mark bottom right on every board. Move the piece count off the Set board face into the data panel. Before Phase 0 asks Chris, write a short brief for the mark and glyph: no letters on studs, no red field, no rounded bold wordmark, and the glyph is a shape. Have the Creative Director sign that each option doesn't resemble the toy maker's logo, checked against a written description, not an image.

### 14. The fan scan is undefined, and its own word lists carry the names

**Where:** ENGINEERING section 4 (`studio fan-scan`); BUILD section 2 (`catalog/banned.json`); CLAUDE_MAPPING (`studio-charter` carries FAN_RULES.md); CREATIVE_DIRECTION, "Direction" quote.

**Problem:**
- The kit gives no method for finding a logo in a render, and no planted-fault test for it.
- The banned-terms list and `banned.json` have to contain the film, theme and character names they block. If skills cite them, those names land in agent text.
- `studio-charter/reference/` carries FAN_RULES, whose disclaimer names the toy maker. If the clay habit of copying every studio doc is followed, it also carries Chris's quoted direction, which names the toy maker and its first film.
- Image models are a known source of fake wordmarks (research/06 section 2), and the `concept/` folder isn't scanned.

**Fix:** Define the scan in three layers and prove each on planted faults. Scene graph: no logo stud primitives, no banned part IDs. Text: board JSON, file names and captions against the terms list. Pixels: OCR on renders, boards and `concept/`. Keep the terms list as data that only the scan reads, outside `skills/`. In the charter's copy of CREATIVE_DIRECTION, replace the quote with "the brick-built feature films and official sets". Keep the disclaimer in NOTICE and the README only.

### 15. "Never sent to an API" conflicts with Claude agents looking at the images

**Where:** KICKOFF ground rules ("never go to an API or a hosted page"); KICKOFF Phase 1 ("A private visual study of real building instructions"); GOLD_SET section 3 ("for the Creative Director's eyes only").

**Problem:** The Creative Director and the Phase 1 researchers are Claude agents. When they read an image, it goes to Anthropic's API. So the rule as written can't be kept. Also, research/06 section 6 notes that the toy maker's site terms include a "No AI" clause. Pulling instructions or box art from that site for an AI study may break those terms.

**Fix:** Either make the inspiration set and the instruction study human-only (Chris looks and writes notes), or say plainly that Claude reading them is the one allowed API use, after Chris has read Anthropic's data terms. Take any study images from Chris's own boxes and booklets, photographed by him, not from the toy maker's site.

### 16. The read test is a hard gate with no pass rule

**Where:** DESIGN_LANGUAGE section 2 ("compare to the brief"); ART_BOARDS step 6; ENGINEERING section 4 (`studio read-test`).

**Problem:** "Compare to the brief" has no rule. The vision model isn't named; if it's Claude, the makers and the reader share a model family, which clay research/04 warns about. The Art Director writes the brief the board is tested against and can pick a subject that's easy to name.

**Fix:** `build_bible.yaml` lists 1 to 3 must-name nouns with synonyms, approved with the Build Brief. The reader is a non-Claude model. A pass means one of those nouns appears in its five words, matched by code. Calibrate on known-bad boards so the gate catches real misreads. Run it on the render with no board text.

### 17. The gold shoot can't produce exact twins as written

**Where:** GOLD_SET section 1 ("Claude sends 6 to 10 small builds ... Build those exactly").

**Problem:** Claude doesn't know which parts and colors Chris owns. The spec also leaves out clay's lock rules: focus, exposure and white balance locked, room lights off, curtains closed (clay GOLD_SET; research/02 section 5; research/05 section 9 adds RAW). Without them, color fitting and twin matching drift.

**Fix:** In Phase 0, ask Chris for a phone photo of his parts spread out, or a short list. Claude designs the builds only from parts he confirms and logs any swap so the LDraw twin still matches. Add to the shoot spec: tripod, no zoom, focus, exposure and white balance locked, RAW if the phone allows, room lights off, curtains closed.

### 18. The tuning crew writes the held-out briefs

**Where:** GOLD_SET "Sizes and splits" ("written by the Creative Director"); KICKOFF Phase 6.1 (the Creative Director is on the R&D crew); clay CLAUDE_MAPPING (`memory: user` on the Creative Director).

**Problem:** The Creative Director writes the held-out briefs and then tunes the board lane, and its memory carries across sessions. The held-out set leaks into the loop.

**Fix:** Chris writes the 10 held-out briefs (one line each), or a one-time agent with no memory writes them straight into the private repo. The Creative Director never reads them; only the checkpoint agent does.

### 19. Motion cards are promised in the board lane, but motion waits for Phase 8

**Where:** ART_BOARDS section 2 ("Motion card"); CLAUDE_MAPPING (`--type ... motion`); KICKOFF Phase 0.4 and Brave bet 4; vs MOTION section 7 ("after Board Bar") and KICKOFF Phase 8 ("only after I greenlight it").

**Problem:** A board type and an entry-point option depend on an engine that may not be built in this milestone. Chris asked for motion, and the first motion he sees could be months away.

**Fix:** Put two simple motion cards in the board lane in Phase 4: a turntable, and builds-itself from `steps.json` (parts arrive on twos, no figure acting). Both need only stepped keys, not the full engine. Leave figure acting, faces and Brick FX for Phase 8.

### 20. The license story misses the Blender side and the catalog

**Where:** ENGINEERING section 6, Option B ("every GPL tool run as a separate process"); BUILD section 2 (catalog from "pyldraw3 geometry analysis"); ENGINEERING section 2 (`catalog/` in the public repo).

**Problem:**
- research/04 section 10 says a published script that imports bpy is itself GPL-bound. So the plugin's own render code can't be MIT under Option B.
- BUILD section 10 says pyldraw3 (GPL-3.0+) "runs as a separate CLI". But the catalog needs its geometry and contact analysis for every part, which is far easier as a library import. The kit should say which it is.
- The built catalog holds data from LDraw part headers (CC BY) and Rebrickable tables. The Rebrickable terms come from a 2017 page (research/07 section 6, [U]), and the data carries the toy maker's set and color names. It would sit in the public repo.

**Fix:** Default to Option A (GPL-3.0-or-later). It fits a non-commercial fan project and removes the process-boundary work. If Chris prefers B, say that the bpy-side code is a GPL sub-package. Don't commit the catalog; build it locally from the pinned library. Add a Rebrickable credit to NOTICE.

---

## Minor

### 21. Wrong section number for the build bible
CREATIVE_DIRECTION line 3 and STUDIO_DELTA section 3 point to "DESIGN_LANGUAGE.md section 6". Section 6 is "Two looks"; the build bible is section 9. Fix both references.

### 22. Who signs the Build Brief
ART_BOARDS step 2 lets the job go ahead "on the Art Director's call if Chris said 'just make it'". CLAUDE_MAPPING and the clay bible say Chris signs every lock. Fix: make "just make it" a standing waiver Chris records once with `studio decide`, and say so in STUDIO_DELTA section 5.

### 23. bpy size and the shared environment
Brick ENGINEERING section 3 says "about 880 MB" (matches research/04 section 9). Clay ENGINEERING section 3 says "about 1.2 GB". Brick also says "one environment can serve both", but clay installs bpy under its own plugin data folder. Use the research number in both kits, and name one shared environment path if sharing is the plan.

### 24. Two pixel filter ranges
LOOK_DEV section 3 says 1.5 to 2.0 px; section 8 says 1.8 to 2.2 for moire. Pick one range (1.8 to 2.0 fits both).

### 25. Research claims overstated, or [U] tags dropped
- BUILD section 3: the validator took models "from rarely valid to almost always valid". research/06 section 4 says the top model was valid on 40% of prompts without the toolchain. Say "from under half valid".
- LOOK_DEV section 4: the Set look wear budget (1 in 10, under 5%, 10% unseated) isn't in research/05. Mark it [U].
- FAN_RULES section 5 and BUILD section 2: Rebrickable data is "free for any use". research/07 section 6 marks this [U].
- BUILD section 4: Gurobi is "free only for academics". research/04 section 6 marks this [U].
- ART_BOARDS section 1: "no tool reliably keeps a stud grid through a restyle". research/06 section 3 says this was never tested. Say "no tool has been shown to".
- STORY_AND_SOUND: "Story is judged as much as animation". research/02 section 1 says that ranking was inferred from award categories [U].
- ENGINEERING section 3 calls all the Blender 5.2 traps "verified". The 5.0 ID property change came from release notes, not a test.
- MOTION rule 6: brick blur lasts "one or two exposures". No source gives a length. Mark it [U].
- KICKOFF Phase 5.2: the "brick style" outputs "double as the bake-off in research/06 section 8". That bake-off mainly asks whether restyle and video-to-video keep a stud grid. Prompt-only outputs answer only part of it. Say "part of the bake-off", or run it separately.

### 26. The figure rig has no waist
MOTION section 3 gives head, shoulders, wrists and hips. research/01 section 3 and research/09 section 1 also list the waist, and the research/04 section 8 rig has a separate hips bone. MOTION rule 11 needs a "torso shift" for a shrug. Add waist yaw to the rig, to `joint_limits.json` and to the shoot's figure lineup.

### 27. Skin tones
DESIGN_LANGUAGE section 4 says to use "the eight real skin colors". research/03 section 2 found no LDraw codes for three of them. Skin is also the head's part color, not part of the print. Reword, and have Phase 1 find the missing codes.

### 28. Skill names that are too close
`board` (the entry point), `boards` (inherited clay storyboards), `board-layout` and the `board-batch` workflow. The clay kit already had one name clash (`dailies`). Rename the inherited skill `storyboards`.

### 29. SessionStart does too much
ENGINEERING section 5 runs the full brick smoke test at every session start. Clay's SessionStart already renders one frame; the brick test adds four imports, a 300-part model and a golden-frame diff on top. Hook timeouts are short by default [verify]. Keep SessionStart to the clay check plus one brick import, and run the full test in CI and Phase 0.

### 30. EEVEE and Workbench without a GPU
research/04 section 7 found EEVEE unusable on a box without a GPU. LOOK_DEV section 8 puts thumbnails, the read test and the instruction look on Workbench or EEVEE, and the board golden test runs in CI. Test Workbench headless in Phase 0 and keep a Cycles fallback for the instruction look.

### 31. Importer defaults vs the look spec
ImportLDraw's gap defaults to 0.1 mm and its bevels are baked "as wide as will fit" (research/04 section 2, research/05 section 1). LOOK_DEV wants 0.15 mm gaps and 0.12 to 0.20 mm bevels. BUILD section 8 picks ImportLDraw "for its richer materials" while LOOK_DEV builds house shaders. The smoke-test bounding box also changes when gaps are on. Name the importer settings in ENGINEERING section 3, and say that house shaders replace the imported ones.

### 32. Template path
KICKOFF Phase 4.10 says "the clay templates/agent-template.md". Write `../clay-motion/templates/agent-template.md`.

### 33. Hidden judge rule
The brick /goal says the hidden judge must be "within 0.05 of the panel". Clay says "no more than 0.05 below". Use the clay rule unless there's a reason for a two-sided one.

### 34. Where board jobs live
Board jobs go in a separate private repo, but nothing tells `/brick-studio:board` where that repo is. Have Phase 0 write a small config file with the path to the boards repo.

### 35. Gaps against what Chris asked for
- "Scene rendering" has no product of its own. Define a scene: a validated set with saved camera and light presets that any board or shot can reuse.
- Set boards need a "themed backdrop" and style frames need a world, but generated plates are banned and no brick backdrops are planned. Add a small backdrop module library (sky, street, room) and a plain studio sweep.
- There's no visual sheet for the design language itself. Make it the first board: one model in set look and film look, the palette chips, the scale ladder, and good and bad examples from the failure checklist.

### 36. Sharing honestly in fan spaces
FAN_RULES section 4 adds "Made with AI assistance" only when a generated image, video or voice is visible. Every board is made by an AI crew, and research/02 section 1 and research/06 section 6 both say fan spaces react strongly to unlabeled AI. Add a plain footer line for anything posted publicly. When family figures appear, keep the kids' names out of titles and captions.

### 37. Writing: the worst offenders
- CREATIVE_DIRECTION, "Toy physics, toy motion": "Parts never bend. Figures move only at their joints. Motion steps like stop motion." A run of short punchy lines. Try: "Parts stay rigid, figures move only at their joints, and poses step like stop motion. Effects come from swapping pieces."
- CREATIVE_DIRECTION: "Photographed, not rendered." Everything here is rendered. Try: "Looks like a macro photo."
- CREATIVE_DIRECTION threads 2 and 3: "Warmth over polish." and "Polish over warmth." Try: "This trades polish for warmth" (and the reverse).
- KICKOFF: "Buildability is code, never opinion." and "The library is the contract." STUDIO_DELTA section 1: "The rule came first and tools enforced it." These read as slogans. Try: "Only the Build Validator decides if a model can be built" and "Agents pass each other LDraw files, part IDs and JSON."
- DESIGN_LANGUAGE section 1: "Ideas don't get decorated with bricks; they get rethought in bricks". Try: "Design each idea in bricks from the start."
- LOOK_DEV section 4: "removable in an A/B without the frame turning CG-clean". Hard to parse. Try: "No single wear layer should carry the look. Turning one off should still leave a frame that looks real."
- LOOK_DEV section 8: "Every effect should be visible only when toggled off." Try: "Keep each post effect subtle enough that you notice it only when it's gone."
- Long sentences: LOOK_DEV section 1 is one sentence of about 90 words; KICKOFF line 15 has one of about 70; README line 3 is about 50 words followed by "Claude agents are the crew." Split each into two or three sentences.
- STORY_AND_SOUND: "Owned, free and better than library sounds." "Better" is a guess. Try: "We own them, they cost nothing, and they match our bricks."
- No em or en dashes anywhere in the kit.

### 38. The writing rule for agents
KICKOFF "Writing for me" bans em dashes only. Add en dashes and the patterns in finding 37, so STATUS.md and gate summaries follow the same rules.

---

## Spot checks: 30 claims against the research

| # | Claim (kit location) | Research | Result |
|---|---|---|---|
| 1 | 17,164 parts in the 2026-09 library (BUILD 2) | research/04 sec 1 | Matches |
| 2 | Part 3001 box is 32 x 11.2 x 16 mm (ENGINEERING 3) | research/04 sec 1 | Matches |
| 3 | 3001 has 8 studs and 19 typed contacts (ENGINEERING 9) | research/04 sec 4 | Matches |
| 4 | 48 KB per linked object, 1.7 KB per instance, 5.2 GB and 2.2 GB (BUILD 8) | research/04 sec 7 | Matches |
| 5 | A 2x4 runs from 248 to over 10,000 vertices (BUILD 8) | research/04 sec 7 | Matches |
| 6 | ldr_tools defaults to a logo stud (BUILD 2) | research/04 sec 1 | Matches |
| 7 | Tool versions and licenses (BUILD 10) | research/04 sec 2 to 5 | Matches |
| 8 | bpy 5.2.2, about 880 MB, Python 3.13 (ENGINEERING 3) | research/04 sec 9 | Matches (clay says 1.2 GB) |
| 9 | Every Blender 5.2 trap "verified" (ENGINEERING 3) | research/04 sec 9 | Partly: one is from release notes |
| 10 | Validator lifted models from "rarely valid" (BUILD 3) | research/06 sec 4: 40% valid without it | Off |
| 11 | Opus 5.5 nearly always valid with the toolchain (CLAUDE_MAPPING) | research/06 sec 4: 0.99 | Matches |
| 12 | Roughness 0.16 and 0.28, IOR 1.53, subsurface weights (LOOK_DEV 2) | research/05 sec 1 | Matches |
| 13 | A 0.2 mm gap is 4 px at 1:5; a stud is 170 px at 1:3 (LOOK_DEV 3) | research/05 sec 2 | Matches |
| 14 | Camera 70 mm away, 24 mm lens, 4 mm depth of field at f/8 (LOOK_DEV 5) | research/05 sec 4 | Matches |
| 15 | Jitter offsets, yaw, tilt and unseated shares (LOOK_DEV 3) | research/05 sec 2 | Matches |
| 16 | Set look wear budget (LOOK_DEV 4) | not in research/05 sec 3 | Off: untagged |
| 17 | 12 bounces, diffuse 6 to 8 (LOOK_DEV 6) | research/05 sec 5 | Matches |
| 18 | Figure scale 1:42, vehicles 1:38, buildings 1:44 to 1:48 (DESIGN_LANGUAGE 3) | research/03 sec 3 | Matches |
| 19 | Mosaics need 48x48 studs (DESIGN_LANGUAGE 3) | research/08 sec 1 | Matches |
| 20 | Starter LDraw color codes (DESIGN_LANGUAGE 4) | research/03 sec 2 | Matches |
| 21 | Eight real skin colors ready to use (DESIGN_LANGUAGE 4) | research/03 sec 2: three lack codes | Partly |
| 22 | Commons categories hold 47 and 31 files (GOLD_SET) | research/09 sec 4 | Matches |
| 23 | Rebrickable data "free for any use" (FAN_RULES 5) | research/07 sec 6: [U] | [U] dropped |
| 24 | Gurobi free only for academics (BUILD 4) | research/04 sec 6: [U] | [U] dropped |
| 25 | No tool keeps a stud grid through a restyle (ART_BOARDS 1) | research/06 sec 3: untested | Overstated |
| 26 | Story judged as much as animation (STORY_AND_SOUND) | research/02 sec 1: inferred | Overstated |
| 27 | A festival bans AI assets but allows Blender renders (STORY_AND_SOUND) | research/02 sec 1 | Matches |
| 28 | The fan policy bars the name in addresses (FAN_RULES 2) | research/07 sec 4 | Matches |
| 29 | Figure joints are head, shoulder, wrist, hip (MOTION 3) | research/01 sec 3: also waist | Off |
| 30 | Concept sketches cost a few cents (ART_BOARDS 6) | research/06 sec 5 | Matches |
