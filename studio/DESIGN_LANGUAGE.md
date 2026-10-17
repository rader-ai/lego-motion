# The brick design language

Owners: the **Creative Director** holds these house rules across productions; the **Art Director** applies them to each production through its build bible (section 9). Sources: research/08 (brickification), research/03 (set design language), research/01 (feature-film craft). Values marked [U] are proposals to test, not facts.

## 1. The language in one paragraph

Every idea becomes a small world you could actually build. It uses real parts in real colors, a short palette, one clear silhouette and visible studs, with a story you can read in one second and a little wear from being played with. It looks like a toy photographed on a table and moves like a toy in a hand. Design each idea in bricks from the start, the way a good builder would, rather than covering a finished idea in bricks.

## 2. Brickification: the method

Every board, scene and film starts here. The Art Director runs it; the output is `build_brief.md`, which Chris can approve in one tap. Ten steps, each with a check (research/08 section 9).

| # | Step | Check |
|---|---|---|
| 1 | **Brief.** The idea in one sentence, plus who it's for and the mood. | A stranger can paraphrase it. |
| 2 | **The question.** What must the image answer: what is it, who's in it, what's happening? This sets the smallest thing the viewer must recognize. | Written down in one line. |
| 3 | **Scale and mode.** Pick the scale from section 3 by that smallest thing; pick system or sculptural building (section 5). | One hero scale per frame. A second scale needs a stated reason (background, joke, transition). |
| 4 | **Silhouette.** Block the shapes in 2 to 3 flat values. | Filled solid black at thumbnail size, the subject is still nameable. |
| 5 | **Palette.** Up to 3 hues, one grounding dark, one accent, a muted backdrop (section 4). | Every color exists in the parts that need it (Brick Librarian checks). |
| 6 | **Seed and parts.** One seed part that anchors the design, one repeatable module, 5 to 10 hero parts for curves and details. | Every part has a real way to attach. |
| 7 | **Story beat.** One character or one moment, with a motive. Focal point off center or on a diagonal. | You can say what happened a second ago. |
| 8 | **Detail budget.** Add only details that serve the read or the story, plus one small gag. | Removing a detail doesn't improve the read. |
| 9 | **Physics.** Clutch, support, no impossible joins. | Build Validator passes. |
| 10 | **Read test.** One-second glance, then thumbnail, then "what is this about" from someone (or a model) that hasn't seen the brief. | If it fails, simplify. Never fix a failed read by adding. |

The one-second read and the black-fill silhouette are our own proposals; no source defines them for bricks (research/08 gaps). How the read test runs (`brick read-test`):
- At Build Brief, the Art Director lists 1 to 3 **must-name nouns** with synonyms in `build_bible.yaml` (for a coffee shop: "coffee shop, cafe, coffee bar"). Chris approves them with the brief, so the subject can't be swapped for an easier one later.
- A 256 px thumbnail of the **hero render with no board text** goes to a **non-Claude** vision model (the makers are Claude, and judges favor their own family, clay research/04) with one prompt: name what this is in five words.
- Pass when code finds a must-name noun or synonym in the answer. The black-fill silhouette version runs too and is logged.
- Calibrated in Phase 5 on known-bad boards (muddy silhouettes, wrong subjects) so the gate catches real misreads. Results go in `read_test.json`.

## 3. Scale

| Scale | Figure height | Studs per real metre | Use it for | What survives |
|---|---|---|---|---|
| **Micro** | 1 brick stack or 1x1 round = a person | under 1 | Cities, landscapes, many buildings at once | Silhouettes, one part per object, repeated modules |
| **Figure scale** (house default) | about 40 mm (1:42 by height; vehicles often closer to 1:38, buildings 1:44 to 1:48) | about 3 | People doing things: rooms, shops, vehicles, vignettes | Story props, poses, printed faces |
| **Large figure** | about 1:17 to 1:20 [U, sources disagree] | about 6 | Crowds of personality, close observed detail | Specific props, small personality gags |
| **Sculpture** | the subject itself, built big | n/a | One object, animal or bust as the hero | Surface texture, color shading, expression |
| **Mosaic** | flat, 48x48 studs minimum to read | n/a | Images, portraits, data | Shapes and color fields; fine detail is lost |

- One stud is 8 mm, one brick is 9.6 mm tall, one plate is 3.2 mm. LDraw works in LDU: 20 per stud, 24 per brick, 8 per plate, 1 LDU = 0.4 mm (research/03 section 3; verify against the pinned LDraw spec in Phase 0).
- Doors and windows size to a figure. Round real dimensions up to the next brick or plate.
- Building and rendering share one scale per scene file, or parts visibly disagree.

## 4. Palette

**House rules**
- Only colors that exist in the pinned LDraw `LDConfig.ldr`, and only in parts that really come in that color (Rebrickable part-color data, owned by the Brick Librarian). No custom colors, no gradients, no painted shading.
- Per model: a dominant color (about half the visible area), a secondary, one small high-contrast accent, and neutrals carrying the mass. Most real small sets land around 4 to 8 solid colors [U].
- One grounding dark. A muted backdrop that never competes with the hero.
- Adjacent parts differ clearly in value so edges read. Avoid near twins side by side (black against dark bluish gray).
- Color follows the story: decide what the build should feel like, then pick colors that say it.
- Changes of state (burned, wet, old) snap to the nearest real part color, never a tint (research/01 section 5).

**Which hex values.** LDraw history has two conventions that disagree a lot (red as #B40000 vs #C91A09, blue as #1E5AA8 vs #0055BF; research/03 section 2). Phase 0 pins the current LDConfig; the Look Development Supervisor picks the convention that matches real bricks under our view transform by photographing real bricks next to a color chart (GOLD_SET.md) and records the choice in `LOOK_DEV.md`.

**Starter families** (LDraw codes, research/03 section 2): neutrals 15 White, 71 Light Bluish Grey, 72 Dark Bluish Grey, 0 Black; earths 19 Tan, 28 Dark Tan, 70 Reddish Brown, 308 Dark Brown; primaries 4 Red, 1 Blue, 14 Yellow, 25 Orange, 2 Green; muted 330 Olive Green, 378 Sand Green, 379 Sand Blue, 320 Dark Red, 272 Dark Blue, 288 Dark Green; lights 226 Bright Light Yellow, 212 Bright Light Blue, 191 Bright Light Orange; brights 27 Lime, 322 Medium Azure, 5 Dark Pink, 353 Coral; transparent 47 Clear, 36 Red, 46 Yellow, 43 Light Blue, 57 Orange, 40 Smoke. Skin tone is the color of the head and hand parts, not part of the print. Use the classic yellow or the real skin-tone colors that exist for those parts; research/03 section 2 found no LDraw codes for three of the eight common skin colors (Umber Brown, Sienna Brown, Medium Tan), so Phase 1 finds them. The production decides once.

## 5. Parts and building

- **System building** (on the grid, studs up, bricks and plates, modular walls) for architecture, interiors, figure-scale vignettes and mosaics. **Sculptural building** (sideways studs, angles, hinges, slopes) for animals, people, vehicles and anything organic [U, our decision rule].
- **Studs show by default.** Tiles are a choice: floors, roads, screens, water, clocks. A model with no studs reads as a smooth CG toy.
- **Detail lives in parts,** not in prints: clips, bars, 1x1 rounds, slopes, and a part used for a job it wasn't made for (a leaf as a column top, a plate as a ripple). Prints are rare and placed for the hero angle. Stickers rarer still.
- **Seed part.** One part that already looks like the key feature anchors the design.
- **Sturdy, like a real set:** walls at least a brick thick, offset stacking, no unsupported spans, interiors never empty.
- **A play feature or a reveal:** a removable roof, a hinge, a hidden room. It gives boards a second image and films a gag.
- **The hardest part first.** Design the roof, the face or the curve before the easy mass.
- Exotic, retired and metallic parts only when the brief needs them.

## 6. Two looks

The films' own theme is improvised building against following the instructions (research/08 section 5). That gives the studio two looks; each production picks one in its build bible.

| | **Set look** | **Film look** |
|---|---|---|
| Feels like | A real set, straight from the box | A world someone built and played in |
| Palette | Closed, tidy, theme-like | Mostly closed, plus a few odd colors in hidden spots |
| Parts | Current, common parts | Anything in the library, mixed eras allowed |
| Wear | Light: fresh plastic, faint handling marks | Played-with: scuffs, dust in corners, a cracked piece somewhere |
| Light | Soft studio key, clean backdrop | Practical light, motivated, cinematic |
| Lens | Moderate depth of field, hero three-quarter angle | Macro, shallow depth of field, a camera with weight |
| Extras | Instructions exist; a play feature | Everything is bricks, including water, fire and sky |

Chris's open choice of thread (CREATIVE_DIRECTION.md) sets the house default.

## 7. Story inside a build

- Plan the story before the parts. Decide whether people or the place is the subject.
- Give every figure a motive: what they're doing, where they're going, why.
- One viewing angle for vignettes, chosen like a photograph. Capture the moment just before something happens.
- One small gag per model: a pet, a hidden tiny model, something slightly wrong in the background.
- Interiors are never empty.
- Story is one of the three things expert judges score, alongside technique and creativity (research/08 section 5).

## 8. Abstract ideas and real people

A render has no builder standing next to it to explain the meaning, so abstract ideas need a visual grammar the viewer can decode (research/08 section 6; mappings are proposals [U]):

| Idea | Brick form |
|---|---|
| Growth, success | Height; a flag or light on top |
| Foundation | The baseplate; what everything stands on |
| Risk | A narrow gap, a loose stud, a tall tower on a small base |
| Connection | A bridge, a shared baseplate, a hinge |
| A team | Figures on one connected base, facing the same thing |
| A process or workflow | A track or conveyor with stations in order, left to right |
| Data | Stacked columns; mosaic pixels at tiny sizes |
| Software | Machinery, or a building with floors you can see into |
| Feedback | A loop: something travels out, comes back and changes the thing it left |
| Emotion | Color plus posture; a figure whose chest opens to spill colored bricks |

One object per idea, color by role, and at most one short caption on the board. If it needs more words, the metaphor is wrong.

**People:** family and friends can appear with their OK, built from a few generic cues (hair color, glasses, a jacket color, one prop), never a claimed likeness. No public figures (FAN_RULES.md). **Places:** 3 to 5 landmark silhouettes, the signature color, one local gag; micro for a city, figure scale for a room.

## 9. The build bible (one per production)

`build_bible.yaml`, written by the Art Director, locked at Build Brief (boards) or Design Lock (films). It's data, because the validator and the read test read it (L2, L8, `brick read-test`); `build_brief.md` is rendered from it for Chris. Every board job has one; a film has one in `bible/`. Fields:
- One-line idea, the one-second read, and the 1 to 3 must-name nouns with synonyms.
- Look: set or film (section 6), and the thread choices that apply.
- Scale, with studs per metre and the figure height.
- Palette: the closed list of LDraw color codes with roles (dominant, secondary, accent, neutrals, backdrop).
- Part vocabulary: seed part, modules, hero parts, parts that are never allowed.
- Budgets: piece count, unique parts, exotic parts, prints.
- Story beats and the gag.
- Prints needed (faces, torsos, tiles), all original.
- Fan-rules check (FAN_RULES.md), signed by the Creative Director.

## 10. Failure checklist

The Critic Panel and the Art Director check every board against this (research/08 section 8, research/03 section 8, research/01 section 8):
- [ ] Too literal: copies every window and logo instead of finding the silhouette.
- [ ] Scale mixing with no reason.
- [ ] Color soup: more than about 5 hues, no grounding dark, no accent.
- [ ] A color or shape that doesn't exist as a part.
- [ ] Part salad: exotic parts with no seed idea or repetition.
- [ ] No story: no focal point, no motive, no moment.
- [ ] Fails the one-second read.
- [ ] Unbuildable: floating, fused, intersecting, unsupported (code catches these first).
- [ ] Blocky approximation with no art direction (the typical look of automatic brick conversion).
- [ ] Studs wrong: missing, wrong count or spacing, flat surfaces with no studs or tiles chosen on purpose.
- [ ] Background fights the hero.
- [ ] Over-explained: captions doing the work the picture should.
- [ ] Smooth CG plastic: no seams, bevels, gaps or wear.
- [ ] Anything from FAN_RULES.md section 2 or 3.
