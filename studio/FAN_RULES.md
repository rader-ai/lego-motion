# Fan rules: what we can use and what stays off limits

Owner: the **Producer**, with the **Creative Director** checking every board and film before it leaves the studio and the fan scan checking in code (ENGINEERING.md section 5). Sources: research/07 (IP and brand rules) and research/06 section 6 (AI and platform rules). This is research turned into house rules, not legal advice.

## 1. The mode we're in

Chris chose **personal and fun only** (Oct 17, 2026). That puts the studio in fan mode:
- Real brick parts from the LDraw library, including standard unprinted figure parts, are fine to build and render.
- Everything stays non-commercial: no ads, no client work, no sponsored posts, no sales of boards, prints or films.
- Sharing with family, friends and public fan spaces is fine with the credits in section 4.
- Monetized channels are a gray area. The toy maker's fan policy only covers non-commercial use and says nothing about video ad revenue (research/07 section 5). Turning on ads counts as a change of mode (section 6).

## 2. Names and marks

| Rule | Why |
|---|---|
| Never use the toy maker's brand name in a plugin, skill, file, folder, film title, channel or board title. Use "brick-built" or "brick". One exception: Chris named the kit repo `lego-motion` (Oct 17, 2026), knowing the policy. The fan scan allowlists git metadata only; new repos and everything inside them follow the rule. | Its fan policy bars the name in web addresses and treats it as an adjective-only word mark (research/07 section 4). |
| Never show the toy maker's logo: not on boards, boxes, signs, props or studs. | The logo may never appear on unofficial material. |
| **Studs never carry the toy maker's logo.** Logo studs are off in every importer, and the fan scan fails any scene that has one. Studs carry our own house glyph in close-ups or stay blank on wide shots (LOOK_DEV.md section 3). | The logo on studs is protected; plain studs are generic (research/07 section 1). Blank studs look wrong up close (research/05 section 2). |
| Our house mark and stud glyph follow a written brief: no letters on studs, no red field, no oval, no rounded bold wordmark, no red, white and black brand look; the glyph is a shape. The Creative Director checks each option against a written description of what to avoid, not against an image of the real logo. | research/07 section 8 lists what an original look must avoid. |
| Say "figure", never the trademarked figure name, in studio text and titles. | The figure name is on the toy maker's trademark list. |
| When anything we share names the toy maker, it carries the disclaimer in `NOTICE`. | The fan policy asks for a disclaimer wherever the word appears. |

## 3. Characters, prints, sets and stories

- **Original only.** Every character, world, vehicle and story is ours, the same rule as the clay studio.
- **No film characters, no licensed themes.** Nothing from the brick films, and nothing from franchises sold as licensed sets. A character built from bricks is still that character (research/07 section 3).
- **Prints are ours.** The toy maker's printed faces, torsos and tiles are its artwork (research/07 section 3), so every printed part in the LDraw library is banned by default. Our Print and Decal Artist makes every face, torso, tile and sticker. The Brick Librarian may allow a truly generic print (a plain clock face, a grille) with a written reason (BUILD.md section 2).
- **No copies of official sets, instruction booklets or box art.** Models are checked against official set inventories in code, and any close match goes to the Creative Director (BUILD.md section 4). Set boards and instruction sheets use our own layouts (ART_BOARDS.md section 4). The toy maker owns copyright in its instructions and catalog photos.
- **Family and friends can be figures** with their OK, built from generic cues (hair color, glasses, a jacket, one prop), never a claimed likeness. Kids' names stay out of titles, captions and anything shared publicly. No public figures.
- **Making text stays clean.** Skill and agent text that drives making cites research file numbers, not real studio, film, set or character names. The banned-terms list the fan scan uses is data that only the scan reads.

## 4. Credits on everything that leaves the studio

- "Brick parts from the LDraw Parts Library (ldraw.org), CC BY 2.0." The LDraw Steering Committee says renders aren't derivative works, but the credit costs nothing, and anything that ships converted part meshes (an exported `.blend` or `.glb`) needs it (research/07 section 6).
- The `NOTICE` disclaimer when the toy maker is named.
- "Made with an AI studio" on anything posted publicly, and "includes generated images" when a generated image, video or voice is visible or audible. Brick fan communities react strongly to unlabeled AI (research/02 section 1, research/06 section 6).

## 5. Tools, data and terms

- **LDraw parts library:** CC BY 2.0, fine for this. The All-In-One installer is CC BY-ND, so install the parts zip, not the installer.
- **Rebrickable data** (part colors and set inventories): its downloads page said free for any use with credit appreciated, but that wording was read from a 2017 page [U]. Phase 1 re-checks the current terms. The built catalog is never committed.
- **BrickLink Studio and Mecabricks:** their render terms weren't fully readable (research/07 section 6). Not in the pipeline.
- **Inspiration material** (film stills, box art, catalog photos) is human-only by default. Claude agents reading an image send it to Anthropic's API, so agents see it only if Chris has read Anthropic's data terms and approves a specific folder. Study images come from Chris's own boxes and booklets, photographed by him, never from the toy maker's website, whose terms include a clause against AI use (research/06 section 6).

## 6. If Chris ever wants commercial use

That's a new decision at a gate, not a drift. What changes (research/07 section 8):
1. An original figure that doesn't share the protected combination (barrel head with top stud, trapezoid torso, C-hook hands, hip block with leg holes). How different is enough is untested; counsel decides.
2. Original names for the figure type, parts, themes and colors, and a repo name without the brand.
3. Every element checked against newer part designs.
4. Counsel answers the ranked questions in research/07 section 9 first.
Until then, fan mode holds.
