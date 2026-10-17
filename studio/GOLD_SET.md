# Gold references: what the judges compare against

Owner: the **Creative Director**. Same purpose and rules as the clay studio's `GOLD_SET.md` (judges need real material to compare renders to; provenance and license logged per item; held-out slice in the private R&D repo). Sources: research/09 sections 4 to 8, research/06 section 7.

## Why this matters even more for bricks

Openly licensed brick reference is thin. Verified Wikimedia Commons categories are small (one vehicles category held 47 files, one figures category 31), no CC0 brick photo set turned up, and no openly licensed brick stop-motion film was found (research/09 section 4). Official set photos and box art belong to the toy maker and stay out of judge APIs, the same way studio stills stayed out in clay.

But bricks have one big advantage over clay: **we can build the gold scene as an LDraw file first.** Every real photo then has an exact digital twin, so the judges compare a render of the same model, from the same angle, under matched light, against the real photo. Differences can only come from the look, not the design (research/09 section 8).

## Sources, in order of preference

### 1. A real brick shoot by Chris (strongly recommended, about 1 to 2 hours)

**What you need:** a box of real bricks and two or three figures (any you have), a phone with a macro or 2x lens on a small tripod, one desk lamp as a hard key, a white card for bounce, a gray card and, if possible, a color chart (any printed X-Rite style chart works for a first pass). A free stop-motion app for the clips.

**Before you shoot:** Claude sends 6 to 10 small builds as LDraw files with parts lists (most from common parts). Build those exactly, so each photo has its twin.

**Stills (about 40, 30 to 40 minutes):**
- Each build from the angle Claude specifies, plus one free angle.
- Macro close-ups: studs on a 2x4, the seam where two bricks meet, an old scratched brick, a fingerprint on a glossy tile, dust in a corner, a transparent part with light through it, a printed face.
- A color row: one brick of each color you have, lined up on the gray card, under the lamp, with the chart in frame. This sets the palette hex convention (DESIGN_LANGUAGE.md section 4).
- A figure lineup: front, side, three-quarter, and the extremes each joint allows (head turn, arm raise, hip swing). These become the joint limit table (research/09 section 6).

**Clips on twos (about 30 to 40 minutes, 12 poses a second):**
- A figure turning its head and blinking (swap two face prints if you have them).
- A walk of 8 to 12 steps across a baseplate.
- Bricks placed one by one to build a small wall (the "builds itself" reference).
- A splash or puff made of transparent bricks moved frame to frame.

**What it gives us:** look-dev targets for plastic, wear, gaps and color; real joint limits; real stepped motion; and the gold set for both stills and motion, all owned by Chris.

### 2. Openly licensed material

- Wikimedia Commons brick photos, license recorded per file. A CC license covers the photographer's rights, not the toy maker's marks or set designs, so use them for look only [U, not legal advice].
- Flickr Creative Commons via the API license filter, per-photo license, author and URL logged [U, not verified this session].
- LDraw's Official Model Repository: known-valid builds, useful as validator test cases rather than as photos. No license stated on the page, so use for internal testing only.

### 3. Private inspiration set (never judged, never uploaded)

Stills from the brick features, official box art and catalog shots for the Creative Director's eyes only, kept on Chris's machine, used as a do-not-copy check. Never sent to a judge API, never on a hosted page.

## Known-bad set

Our own renders with tells switched on (research/09 section 8): no bevels, no jitter or gaps, motion blur, bent limbs, a floating or overlapping brick, studs removed, off-palette colors, smooth volumetric fire and water, fake tilt-shift blur, clean flat lighting. Plus "brick style" prompts from at least 3 image generators and 2 video generators (research/06 section 8), which also fills in the bake-off.

## Sizes and splits

- Stills: at least 40 gold, 40 known-bad, with 10 gold held out in the private repo.
- Motion (after Board Bar): at least 12 gold clips, 12 known-bad, 4 held out.
- Board briefs: 10 tuning briefs and 10 held-out briefs, written by the Creative Director, covering a place, a person's job, a vehicle, a creature, an abstract idea, a process, a memory, a joke, a product and a sport.

## Raters

Same as the clay studio: Chris at checkpoints; 3 to 5 outside raters at Board Bar and Studio Bar. For bricks, at least one rater should be an adult who builds with bricks as a hobby, because they spot impossible builds and wrong parts that most people miss.

## Data use

Check each judge provider's API data terms before sending any gold item. Chris's own photos are his to send once he's comfortable with the terms. The private inspiration set is never sent anywhere.
