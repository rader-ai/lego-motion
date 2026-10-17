# Gold references: what the judges compare against

Owner: the **Creative Director**, with the **DP** owning camera matching. Same purpose as the clay studio's `GOLD_SET.md`: judges need real material to compare renders to, provenance and license are logged per item, and the held-out slice lives in the private R&D repo. Sources: research/09 sections 4 to 8, research/05 section 9, research/06 section 7.

## Why this matters even more for bricks

Openly licensed brick reference is thin. Verified Wikimedia Commons categories are small (one vehicles category held 47 files, one figures category 31), no CC0 brick photo set turned up, and no openly licensed brick stop-motion film was found (research/09 section 4). Official set photos and box art belong to the toy maker and stay out of every judge.

Bricks do have one big advantage over clay. **We can build each gold scene as an LDraw file first,** so every real photo gets an exact digital twin. After the camera is matched, the judges compare our render of the same model from the same angle against the photo, and most of what's left to differ is the look (research/09 section 8). Some differences will still come from match error, part mold versions and real wear, which is why there's also an open realism test (ENGINEERING.md section 10).

## 1. Chris's brick shoot (strongly recommended, about 2 to 3 hours over one or two sessions)

**Before the shoot**
- Chris sends a phone photo of his parts spread out (or a short list of what he has: bricks, plates, slopes, a few figures, transparent pieces).
- Claude designs 9 to 12 small builds (20 to 80 parts each) only from parts Chris confirms, and sends each as an LDraw file with a parts list and a picture. If Chris swaps a part while building, he notes it and the LDraw twin gets updated to match.
- Three of the builds are marked held-out. Their files and photos go straight to the private R&D repo, and no tuning agent ever sees them.

**What you need:** a phone with a macro or 2x lens on a small tripod, one desk lamp as a hard key, a second light setup (a window with a sheer curtain, or a second lamp through paper), a white card for bounce, a gray card, a printed color chart if possible, and a baseplate under every build (its stud grid lets us solve the camera position exactly).

**Lock everything:** tripod, no zoom, focus locked, exposure locked, white balance locked, RAW or the phone's least-processed mode if it has one, room lights off, curtains closed for the lamp setup.

**Stills**
- Tuning builds: each from 3 angles under the lamp setup, plus macro close-ups: studs on a 2x4, a seam between two bricks, an old scratched brick, a fingerprint on a glossy tile, dust in a corner, a transparent part with light through it.
- Held-out builds: each from 5 angles under **both** light setups (3 builds x 5 angles x 2 lights = 30 held-out pairs). The tune loop never sees the second light setup.
- A color row: one brick of each color Chris has, on the gray card, under the lamp, with the chart in frame. This fits the palette and the hex convention (DESIGN_LANGUAGE.md section 4).
- A figure lineup: front, side, three-quarter, and the limits of each joint (head turn, waist turn, arm raise, wrist twist, hip swing). These become `rig/joint_limits.json`.

**Clips on twos** (12 poses a second, about 30 minutes): a figure turning its head and blinking (two face prints if available); a walk of 8 to 12 steps across a baseplate; a small wall built brick by brick; a splash or puff of transparent bricks moved frame to frame.

**Sounds** (10 minutes, STORY_AND_SOUND.md): snaps, clicks, drops and rattles on a hard table, phone close.

**Processing:** every gold photo goes through one fixed, written-down step (the same raw conversion, no extra sharpening or noise reduction) before any judge sees it, so judges can't tell a render from a photo by phone processing. A code check measures sharpening halos and noise level on our renders, so the loop can't win by faking phone processing either.

**Camera match (DP):** for each twin, read focal length from the photo's metadata, then solve the camera pose from the baseplate's stud grid. Pass when the reprojection error is under 2 pixels [U, starting threshold]. Twins that don't match are dropped, not judged.

## 2. Openly licensed material

- Wikimedia Commons brick photos, license recorded per file. A CC license covers the photographer's rights, not the toy maker's marks or set designs, so these are used for look only [U, not legal advice].
- Flickr Creative Commons via the API license filter, per-photo license, author and URL logged [U, not verified].
- LDraw's Official Model Repository files go only into the validator's test folder (BUILD.md section 3), never into authoring or judging.

## 3. Private inspiration and study material

- **Human-only by default.** Stills from the brick features, box art and catalog shots are for Chris's eyes. Claude agents reading an image send it to Anthropic's API, so this material isn't shown to any agent unless Chris has read Anthropic's data terms and says so for a specific folder.
- Instruction and box conventions (KICKOFF Phase 1): Chris photographs his own boxes and booklets. Nothing is pulled from the toy maker's website, whose terms include a clause against AI use (research/06 section 6).
- Never on a hosted page, never sent to a judge.

## 4. Known-bad set

Our own renders with tells switched on (research/09 section 8): no bevels, no gaps or jitter, motion blur, bent limbs, a floating or overlapping brick, studs removed, off-palette colors, smooth volumetric fire and water, fake tilt-shift blur, flat clean light, fake phone sharpening. Plus "brick style" outputs from at least 3 image models, which also covers the prompt-only part of the generative bake-off (research/06 section 8).

## 5. Sizes and splits

| Set | Tuning | Held out |
|---|---|---|
| Twin pairs (stills) | 6 to 9 builds, about 20 to 30 pairs, lamp setup | 3 builds, 30 pairs, both light setups |
| Open realism (board heroes vs random gold photos) | as many as rounds need | 20 pairs |
| Board briefs | 10, written by the Creative Director | 10, written by Chris (one line each) or by a one-time agent with no memory, straight into the private repo |
| Motion clips (after Board Bar) | 8 | 4 |
| Known-bad stills | at least 40 | n/a |

- With 30 held-out pairs, a 0.40 win rate carries a 95% interval of roughly plus or minus 0.18. Reports always show the interval.
- The Creative Director and every tuning agent never read the held-out briefs or builds; only the checkpoint agent does.

## 6. Raters

- Chris rates at checkpoints and pilots, but **his votes on his own photos don't count toward the bar,** because he can't be blind to them. For twin realism, the stop guard and the bar use outside raters.
- 3 to 5 outside raters at Board Bar and Studio Bar, at least one an adult who builds with bricks as a hobby (they spot wrong parts and impossible builds that most people miss).
- At least one outside rater joins Phase 5 calibration, so "human majority" means more than Chris.

## 7. Data use

Check each judge provider's API data terms before sending any gold item. Chris's own photos are his to send once he's comfortable with the terms.
