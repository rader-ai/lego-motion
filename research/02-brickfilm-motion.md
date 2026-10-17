# Brickfilm motion craft (research, Oct 17, 2026)

Method: about 38 searches and 60 fetches, then the search budget ran out and the fetch proxy began rate limiting (429), so some pages were never read (listed under Gaps). Page text came through a summarizing fetcher, so any number that will drive code should be re-checked at the source. Brickfilm makers publish almost no frame counts; most numbers below are walk-cycle pose counts, plate steps and CG-production facts. Counts for head turns, blinks, mouth shapes and sound are mostly [U] (inferred from the sister clay doc 07-motion-craft.md plus the sources). All counts at 24 fps unless stated.

## 1. History and the community quality bar
- Origins: a c.1960 German LEGO ad is the earliest known brick animation; "En rejse til maanen" (1973, Lars C. and Henrik Hassing, Super 8, made for their grandparents' anniversary) is the first known fan brickfilm (en.wikipedia.org/wiki/Brickfilm; acmi.net.au brick-films-through-ages). It reached YouTube only in 2013.
- The Magic Portal (Lindsay Fleay, Perth, 1985 to 1989, 16 mm Bolex, about 16 min, roughly four years, mixed with plasticine and live action). LEGO's lawyers briefly blocked it. Fleay later worked at Animal Logic, and the LEGO Movie's "Magic Portal" tube in the live-action ending is a nod to him (thebrickblogger.com 2017 fan-films piece; Wikipedia).
- Web era: Brickfilms.com (Jason Rowoldt, coined "brickfilm"; founding date differs by source, Dec 2000 vs Mar 2001). Bricks in Motion (BiM) founded 2001, relaunched 2008 by Jonathan Schlaepfer, now the main hub (forum, Discord, contests, Patreon, "World of Brickfilms" podcast).
- Pro-grade early work: Spite Your Face Productions (Tony Mines, Tim Drage, London): Monty Python "Camelot" for the Holy Grail DVD, Spider-Man "Peril of Doc Ock", "Han Solo Affair". They used digital animation for some facial expressions (archive.org blog, 2007).
- Michel Gondry's White Stripes "Fell in Love with a Girl" (2002): footage was pixelated and rebuilt as 24 brick scenes per second of video (so effectively ones at 24 fps), with roughly half the video touched digitally (elephant.art). LEGO declined to partner; the band bought the bricks.
- Makers and films worth studying (community and press picks): Maxime Marion "Henri & Edmond" (called the most cinematic: clean shots, fades, lighting, score); JamesFM "The Letter" (praised for smooth animation and gags); Plastic Planet "The Dandelion" (writing beats resources, 4.2M views); Blunty3000 "Circle Circle Dot Dot" (17.5M views); Garrett Barati "Play Nice"; Marc Beurteaux "Robota"; Mirko Horstmann "Greedy Bricks" (only 10 elements); Rymdreglage "8-bit Trip"; David Pagano "Little Guys!" (giant brick-built actors); Stefan van Zwam (classic era, made the free masking tool Blue Screen of Life). Sources: Make: (David Pagano's picks), Paste, Know Your Meme (weak source), Brickfilms Wiki.
- BRAWL (one-week contest) winners are a ready study list: Ben Boatwright 2008, Gregory and Garry Moore (2012, 2015, 2019), Harrison Allen 2013, Ahmari Ly-Johnson 2016, Christof Munoz 2017, Ben Young 2018, Keith and Stuart Nelson 2020, William Osborne 2021. Entries grew from 11 (2008) to 154 (2023). THAC is the 24-hour sister contest. A 3 to 6 judge panel submits ranked ballots (Brickfilms Wiki).
- Festivals: Brickworld Film Festival (13th edition June 20 2026, Schaumburg IL; 30 s to 5 min 30 s; LEGO must be the main visual medium; Blender and Stud.io renders are allowed; AI-generated assets are banned; awards Judges' Choice, Attendees, International, Best Sound, Best Technical Animation, Best Screenplay, Best Production Design, Youth). Steinerei (Germany/Austria, since 2005, three-person jury plus maker and audience prizes, 20th edition June 8 2024 Vienna). Bricks by the Bay 2014 (Best Film, Best Story, Best Technical Achievement; juror David Pagano). Also Cine Brick (Portugal), Ciné Brique, Brickfilmers Guild festival, Brickfilm Day (since 2018).
- What judges reward, read from category lists: BAMPA 2007 (Best Film, Animation, Cinematography, Screenplay, Sound Design, Score, Visual Elements, Voice Acting) and the planned GO Film 2013 criteria (story, theme, cinematography, animation, sound, set building, editing) show animation is one axis of seven or eight; story, sound and set design count as much. Pagano's own picks stress economy and design over brick count. [U] That ranking of priorities is inferred from category structure; no jury has published a rubric.
- Studio risk: Brickworld bans AI-generated assets. Brick Studio output built from code (Blender renders) is allowed as a render, but "AI crew" framing would need care for festival entry.

## 2. Frame rates and cadence
- Community default: 12, 15 or 24 fps (Wikipedia). Stop Motion Studio defaults to 15 fps and an old Eurobricks tutorial calls it the most common; beginner guides say 8, 12 or 15 (brickfact.com), 12 as standard (garagefarm.net), 12 for a classic look and 24 for smooth at twice the frames (darkskiesfilm.com). Higher fps means each move per frame must shrink (Eurobricks).
- Ones vs twos: ones = 1/24 s per pose, twos = 1/12 s. Twos at the same speed double the distance per pose and strobe on fast moves; twos suit small moves or a stylized look (stopmotionanimation.com). A 12 fps brickfilm is effectively "twos" on a 24 fps timeline.
- CG features: The LEGO Movie animated on twos and threes (motionpictures.org); LEGO Batman Movie on twos with fives in some scenes, and animators were told never to let the computer interpolate between poses (Cartoon Brew, McKay Q&A).
- Mixing: the sister doc rule applies [U]: characters on twos, camera and fast action on ones. No brickfilm source documents mixing; no maker interview with explicit ones/twos choices was found.
- Evidence-based counts: Eurobricks walk tutorial uses an 8-pose cycle at 15 fps (about 0.53 s per cycle; 8 poses on twos at 24 fps would be 0.67 s). Other makers describe cycles of 4 (Wikipedia, "standard"), 5 (brickfact.com), or 6 (BrickNerd, 2025) poses. So the cycle length is a maker choice, not a standard.
- Jump: raise the figure one plate per frame, hold a few frames at the top, reverse down, swing arms up and back, add a little anticipation and a shadow (Eurobricks jump thread). One plate is 3.2 mm [U general LEGO dimension]. Even one-plate steps give no easing; vary 2,2,1,1 up and 1,1,2,2 down [U].
- Draft counts at 24 fps on twos, all [U] (derived from the sister doc and the above):
  - walk step (plant to plant): 8 to 12 frames (4 to 6 poses); full 2-step cycle 16 to 24
  - head turn 90 degrees: 8 to 12 frames (4 to 6 poses, 1 : 3 : 1 spacing)
  - arm gesture: 6 to 8 frames
  - jump: anticipation 4, rise 6 to 8, apex hold 2, fall 6, landing settle 4; total about 24 to 30
  - blink (replacement face): closed pose 2 frames, total 2 to 4; a minifig has no half-lid, so skip long blinks
  - holds: multiples of 2; avoid 3 to 5 frame holds (reads as a mis-shoot per the sister doc); 12 or more for an expression to register
- Scale note: tiny figures should move fast. A Eurobricks poster argues a minifig walk looks quick at 15 fps because the figure covers the same ground per second as a scaled person; do not slow it to look cute.

## 3. Motion grammar for a rigid figure (head, shoulders, wrists, hips)
- Ease: smallest step, grow to the midpoint, shrink to a small last step (BrickNerd 2025, firestartoys); slow near the peak of a move (brickfact.com). The sister doc's surface-gauge ratio (roughly 1 : 3 : 4 : 3 : 1) is a usable starting curve [U for bricks].
- Anticipation without squash: move slightly the opposite way first, for example lean the torso back before bending down; do the same before arm raises, kicks or strikes (BrickNerd). Overshoot and recovery are taught in the LEGO Animation Book (Pagano and Pickett) under "anticipation, follow-through, recovery", plus overlap and secondary action. The book's index puts ease at pp. 46 to 47 and anticipation at pp. 48 to 49.
- Walk: tie each foot to a stud, step stud to stud, swing the opposite arm, add a one or two frame in-between between studs (firestartoys). Known tells: "pigeon bob" when forward travel happens on only some frames; body dropping when legs spread on a run (a Eurobricks critic says it should rise). Walk body bounce, hip twist and torso tilt are the main life-givers (darkskiesfilm.com). The book has chapters on the basic walk, walk tips and "unusual walks".
- Run, fall, fight, lift, sit [U unless noted]: run = airborne frames with both feet off the ground, body tilted forward; fall = whole figure pivots about a contact point with no limb follow-through; fight = lean back 2 poses, snap the arm on an arc, hold the hit 4 to 6 frames, then recover; lift = pose arms forward at the shoulder and swap in a pose with hands near the load, since there are no elbows; sit = legs raised 90 degrees at the hips, torso drops onto the studs, settle with a small head nod. The book's body language chapter has "basic gestures", "complex gestures" and a community-developed shrug technique (BrickNerd review).
- Emotion: small head movements for shock, big motions for anger or joy (BrickNerd). In the LEGO Movie a shrug is faked by arm move plus torso shift plus head tilt, since there are no collarbones (fxguide). Head tilt, lean and arm pose carry feeling when the face is a fixed print.
- Cheats that brickfilmers and the CG films accept: replacement parts mid-shot (swap an arm, torso or head); torsos with popped-out arms and antenna pieces as "cybernetic" limbs (BrickNerd book review); brick-built limbs and the "PaganoPuppet" for more articulation (book chapters 2 and 5); shaving a neck piece so the head can twist (McKay, Cartoon Brew); hips separated from the torso for splits with the seam kept off camera, and turning a figure away so intersecting arms are not seen when it claps (motionpictures.org); extra bricks added at will, such as a wand piece as a spark (fxguide).
- Dance: Piece by Piece (2024) found dance hard because figures do not bend; animators studied a 24-hour "Happy" video and drew pose guides (vfxvoice). Hold big poses, pop between them.
- Style conflict: motionpictures.org says the LEGO Movie avoided squash and stretch, while an Animation Magazine piece says fast, pulled-off-limb moves showed some squash and ricochet. Treat deformation as a rare hand-made exception, not a rule [U].

## 4. Faces and dialogue
- Three families in the LEGO Animation Book: purist replacement (swap real printed heads), custom replacement (custom-printed or hand-drawn heads and faces), and digital facial animation (faces added in post). Lip-sync prep is a post-production step in the same book.
- Practical swaps: use heads with similar shape so eyes and headpiece line up (BrickNerd); custom-print faces on official heads. Stop Motion Studio can add digital faces: animate with the head turned to a blank face, then place and copy a face layer onto each frame, with eye and mouth shape, opacity and rotation adjustable (huehd.com how-to). A Skillshare student used an After Effects lip layer with Posterize Time so the mouth steps with the figure; the lip sync is 2D only.
- CG practice: the LEGO Movie built a per-character library of expressions swapped frame by frame, used A, E, I, O, U mouth shapes, and even reproduced the bumps and scratches that repeated sticker prints leave (motionpictures.org); faces stay flat printed (Animation Magazine). A facial rig also let animators play subtle beats, e.g. Emmet overhearing coworkers (3D World, 2014). Actors acted out each emotion before animating (McKay).
- Number of mouth shapes: no brickfilm source gives one. The LEGO Movie reportedly used vowel shapes. [U] 6 to 8 printed mouths cover brick speech: closed, small open, wide open, O, wide smile, teeth on lip (F/V), plus smile and frown for emotion. The sister doc says clay films use 8 to 15.
- Timing on twos [U, from sister doc]: change a mouth at most every 2 frames; closed shapes (B, M, P) land on the sound frame or 1 frame before; mouth leads the sound by 1 to 3 frames; head or gesture starts 4 to 6 frames before a line. Record dialogue first and animate to a timesheet (Eurobricks commenter; BrickNerd also finalizes voiceover first).
- Eyebrows and eyes: brickfilm heads have fixed print brows, so brow acting comes from swapping heads or digital overlays. Sister-doc note: Wildwood's printed faces suggest swapping every frame on ones is viable when the budget exists [U].

## 5. Rigging and practical
- Anchoring: tape down baseplates (a Eurobricks poster calls it essential); Blu-Tack on set pieces and under the camera base; sticky putty for figures on non-stud surfaces; skip building anything the camera will not see (BrickNerd, Brick Fanatics).
- Camera and capture: tripod (can be built from LEGO) or a LEGO camera cradle/mount; headphone volume button as remote shutter to avoid wobble; manual mode, fixed white balance and exposure (brickfact, garagefarm). DSLR plus Dragonframe with its controller so the camera is never touched (BrickNerd's Canon Rebel T7 setup). Stop Motion Studio Pro (a GBP 4.99 one-time purchase in 2025) lets users mask out supports (firestartoys). Onion skin and overlay are the standard check for continuity (book index pp. 132, 174; garagefarm; darkskiesfilm).
- Flicker (book index p. 170 has a flicker section): block daylight with curtains, dark clothing to stop reflections, LED bulbs, never light from monitors or phones, diffuse lamps with paper, fixed lights, no auto exposure (BrickNerd, firestartoys, brickfact). Tutorials say plastic shines: tune light angles until no reflection hits the lens.
- Lighting at toy scale: 3 lights (key, fill, backdrop) from desk lamps; camera and key light in different quadrants of a cross drawn through the subject; backlight for a cinematic look; tiny lights from LEGO or third-party lighting for built-in sources (BrickNerd; book chapter 6). The LEGO Movie used small environments with more atmosphere than normal (a Trumbull-style smoke-tank idea, via photographer Vesa Lehtimaki; fxguide).
- Holding mid-air: plates under the feet in ground color, removed per frame by painting out on a clean background plate (Eurobricks jump thread); fishing line (a forum commenter, thebrickblogger); the book has "in-camera techniques" and "advanced levitation" sections. Remove rigs in DaVinci Resolve or After Effects (BrickNerd); Blue Screen of Life by Stefan van Zwam is a free masking tool. The BrotherhoodWorkshop made LEGO's own "make things fly" tutorial.
- Camera dollies and sliders: the book teaches building dollies and rigs from bricks; BrickNerd recommends second-hand sliders. For toy scale, camera moves must be shot in increments, one step per frame [U].
- Scale tricks: micro-scale models for big environments, always followed by a normal-scale shot; forced perspective with minifigure foreground and micro-scale background; Blender with the Mecalizer plugin for printable LEGO landscapes (BrickNerd 2025).
- Cost reality: LEGO's own end-credits stop-motion for the LEGO Movie took 3 animators 2 months for 3 minutes, moving about 150 pieces per frame in the last 12 seconds (Stoopid Stoodios; lego.com). [U] Use this to justify CG over real shoots.

## 6. Props and effects by substitution
- Smoke: LEGO's own tutorial episode 3 covers flying and falling objects, smoke and motion blur; episode 4 covers water and backgrounds (thebrickblogger summaries). The book has "Brick-Built Special Effects" with an "Up in Smoke" exercise (pp. 78 to 79).
- CG precedent: LEGO Movie explosions and train smoke were built from individual bricks; water and clouds are brick-built (fxguide). On LEGO Movie 2, Sweet Mayhem's explosions were hand-sculpted in LEGO Digital Designer and animated by "procedural replacement" so each frame is a different brick sculpture, with almost no particle dust or smoke (vfxvoice, 2019). Destruction follows real brick connections so some chunks stay joined while others fly off (fxguide, 3D World). A jitter system gives bricks the slight angle errors real stacks have.
- Brick blur and speed lines: fast figures get a stretched strip of bricks in the character's colors or 3D speed lines built from bricks (motionpictures.org; filmscalpel).
- Pipeline example: Houdini pyro sim, cache, make points, read temperature, copy bricks to points, shade (sidefx.com tutorial). To get a stop-motion feel, hold each brick arrangement for 2 frames or more and do not interpolate [U].
- Build-itself and disassembly: AnkTales (Andrea) films step-by-step builds; an Elin Roberts LEGO dragon with 479 pieces assembles itself in stop motion (thekidshouldseethis.com). [U] The standard method is to disassemble in reverse and play the footage backwards, because builds are easier to take apart on camera; no source confirmed that.
- Gondry's video used bricks as bodies and scenes rather than minifigs.
- Pieces for effects: translucent orange and red for fire, white and clear for steam, flung pieces on clear rigs for debris [U common practice, not sourced].

## 7. How the CG features imitated brickfilm limits
- Stated mandate: Lord and Miller asked for a film as if a kid in a basement had unlimited money (motionpictures.org). Production designer Grant Freckelton said the aim was to leave the audience unsure whether it was stop motion (Vox via lego.com). Chris Miller's rule: any frame should be buildable for real.
- Kept from brickfilms: twos and threes (no in-betweening); no motion blur (brick blur instead); rigid parts, no bent elbows or knees (a figure moves as a child would push it); characters sitting on studs; a driver whose arms do not reach the wheel was kept on purpose; animation inside real LEGO articulation; flat printed faces with swapped eyes, brows and mouths; sticker artifacts; fingerprints, scuffs and microscope-scanned seam lines; resin-oxidized grime; a jitter on brick angles; minimal depth-of-field proxies.
- Deliberately broken or extended: huge brick counts (3,863,484 unique bricks, about 15 million used; acmi.net.au); camera moves a real toy camera could not make, via a keyframed virtual Steadicam that adds a small nervous wobble and avoids single-axis pivots (fxguide); photoreal lighting (Animation Magazine); facial rig for subtle acting (3D World); brick water, clouds and destruction at a scale impossible by hand; allowed exceptions such as a nod (lego.com). [U] They also did not reproduce amateur flicker, camera wobble or hand-in-frame errors.
- Evolution: LEGO Batman Movie kept the brick limits but animated "more sophisticated" on twos with some fives (Cartoon Brew); later direct-to-video works such as LEGO DC: Shazam! dropped the brickfilm look for fluid, bendy animation (Senses of Cinema).
- Walking: CG figures step stud to stud rather than slide (shown by the stud-seating rule); the sources describe walks, hops or an unseen hand pushing, with no sliding glide [U inference].

## 8. Sound conventions for brick worlds
- Sourced: dedicated sound chapter in the book (types of sound, recording, foley at p. 178, using other people's sounds). Brickfilm Awards value sound as its own award (Brickworld Best Sound; BAMPA Best Sound Design and Best Score). BrickNerd: finalize the voiceover first and cut clips to it; add effects and music last; record voices in Stop Motion Studio Pro; capture app effects (lightsaber, gun) by screen recording. Free libraries named by the book authors: Incompetech, Sound Image, Free Stock Music, SweetSoundEffects.
- Reviewers flag weak sound and dialogue as the main fault in otherwise strong brickfilms (Paste on Gabriel Clifft's Bilbo vs Frodo).
- [U] Conventions to adopt (none were verified in sources): a short plastic "click" on every stud contact (foot plant, hand grip, brick snap), a hollow "clack" for plate on plate, pitch variation per hit so it does not repeat, a louder click for heavy bricks, tiny rattle for debris, and record on a hard table with a close microphone. Real makers record foley by snapping actual bricks. I could not find the LEGO Movie sound team's method.

## 9. Tells of bad brick animation
- Floaty, interpolated motion: CG animators on LEGO Batman were told not to let the computer in-between (Cartoon Brew). Cure: stepped keys on twos.
- Figures bending or stretching: breaks the toy contract; 2024's Piece by Piece shows even dance stays rigid.
- Sliding feet or "pigeon bob": forward travel on only some frames, feet not planted on studs, body dropping when legs spread on a run (Eurobricks critique).
- Jumpiness: bumped camera or table, a single unfixed light, shifting daylight (Eurobricks, firestartoys, BrickNerd).
- Flicker: auto exposure, auto white balance, monitor or phone glow, bright clothing reflecting.
- Stray frames: accidental hand-in-frame and double-shot frames must be deleted before filters (firestartoys).
- Inconsistent cadence: mixing ones and twos on one figure without intent; unexplained spacing jumps or holds of 3 to 5 frames [U, sister doc].
- Mushy timing: no easing, no anticipation, same step size throughout, no holds on key poses (BrickNerd).
- Reflections and plastic glare from badly angled lights (Eurobricks, brickfact).
- Sound mismatch: dialogue and effects weaker than the animation (Paste).
- Automated checks [U]: foot displacement while planted, per-frame luminance variance, count of unique frames per second, pose change at non-multiples of the chosen cadence.

## 10. Draft defaults for Brick Studio [U]
- Default cadence: characters on twos at 24 fps (12 poses/s), camera and fast action on ones; keys constant interpolation; no motion blur; use brick-blur strips for speed.
- No deformation on the body; allow replacement parts and head swaps; a face library with 6 to 8 mouths and 2 to 3 blinks per character.
- Stud-to-stud feet, 8 to 12 frame steps; jump built from plate-height steps with easing; rigs allowed and "painted out".
- Imperfection: fingerprints, seam lines, tiny brick angle jitter on pieces, not on contacts, eye-lines or holds.
- Effects: per-frame brick arrangements for smoke, fire, water and explosions, no particles; builds and disassembly played in reverse.
- Sound: stud click per contact, plate clack, dialogue first.

## Gaps and open questions
- Not read (fetch proxy rate limit, 429): stopmotionstudio.com, dragonframe.com, animationpodcast.com ones/twos page, an Obsidian note on ones/twos/threes, Wikipedia The Magic Portal, imvdb Gondry post, theregister.com Spite Your Face piece, adaptalux.com toy-scale fire and explosion page. Search budget ran out at about 38 searches, so no foley or YouTube-maker sources.
- Missing: frame counts for brickfilm head turns, blinks and mouth swaps from real makers; Dragonframe deflicker and x-sheet specifics; the LEGO Movie sound design method; any official brickfilm festival rubric; whether any brickfilmer routinely mixes ones and twos.
- Worth buying/reading: The LEGO Animation Book (Pagano and Pickett, No Starch, 2016) chapters 2 and 3; watch Brickworld and BRAWL winners listed above; interview or survey of BiM forum posts on walks.

## Sources
- https://en.wikipedia.org/wiki/Brickfilm
- https://www.acmi.net.au/stories-and-ideas/micro-moviemaking-brick-films-through-ages/
- https://www.acmi.net.au/ideas/listen/art-vfx-animal-logic
- https://brickfilms.fandom.com/wiki/Brickfilm_Rapidly_All_Week_Long_Contest
- https://brickfilms.fandom.com/wiki/GO_Film_2013
- https://brickfilms.fandom.com/wiki/Stefan_van_Zwam
- https://www.brothers-brick.com/2007/04/10/2007-brickfilming-achievement-in-motion-picture-arts-nominations-announced/amp
- https://brickworld.com/brickfilm-festival/
- https://brickfilming.fandom.com/wiki/Steinerei
- https://steinerei.de/wp-content/uploads/2024/05/Pressetext-STEINEREI.pdf
- https://www.thebrickfan.com/?p=11464
- https://makezine.com/article/craft/photography-video/david-paganos-brickfilms-picks/
- https://www.pastemagazine.com/movies/lego-the-other-movies-10-great-brickfilm-shorts
- https://knowyourmeme.com/memes/subcultures/brickfilms
- https://blog.archive.org/2007/07/25/stop-motion-part-i-attack-of-the-toys/
- https://elephant.art/michel-gondrys-lego/
- https://nostarch.com/legoanimation and the dTOC/index PDFs (Nostarch.com/download/LEGOAnimation_dTOC_sample.pdf, LEGOAnimation_index_sample.pdf)
- https://nostarch.com/animationresources
- https://bricknerd.com/home/12-tips-to-improve-your-lego-stop-motion-animation-4-4-25
- https://bricknerd.com/home/review-the-lego-animation-book-20-2017
- https://blog.firestartoys.com/how-to-take-your-brickfilms-to-the-next-level/
- https://www.eurobricks.com/forum/forums/topic/22270-old-brickfilming-tutorial/
- https://www.eurobricks.com/forum/forums/topic/22317-old-brickfilm-technique/
- https://www.eurobricks.com/forum/forums/topic/32986-old-brick-flick-technique-jumping
- https://www.eurobricks.com/forum/forums/topic/113184-looking-for-a-rig-for-a-lego-minifigure-using-blender
- https://brickfact.com/blog/guidebooks/lego-stop-motion-animation
- https://garagefarm.net/blog/brickfilms-stop-motion-animation-in-the-world-of-3d
- https://darkskiesfilm.com/how-to-make-lego-3d-animation/
- https://stopmotionanimation.com/11/ANIMATION-BASICS
- https://huehd.com/ufaqs/how-to-add-digital-faces-to-lego-minifigures/
- https://www.skillshare.com/projects/217662
- https://thebrickblogger.com/2017/06/how-fan-films-shaped-the-lego-movie/
- https://thebrickblogger.com/2017/01/simple-lego-brickfilming-tips-tricks/
- https://thekidshouldseethis.com/post/a-lego-dragon-self-assembles-with-stop-motion-animation
- https://www.motionpictures.org/2014/02/how-the-lego-movie-got-its-stop-motion-look/
- https://www.fxguide.com/fxfeatured/brick-by-brick-how-animal-logic-crafted-the-lego-movie/
- https://www.fxguide.com/?p=48503
- https://animationmagazine.net/?p=180487
- https://www.pressreader.com/australia/3d-world/20140520/281724087577080
- https://www.lego.com/categories/adults-welcome/article/was-the-lego-movie-stop-motion
- https://www.cartoonbrew.com/?p=148431
- https://vfxvoice.com/?p=7435387
- https://vfxvoice.com/the-building-blocks-behind-the-lego-movie-2/
- https://vfxvoice.com/dancing-to-the-animated-beat-of-piece-by-piece/
- https://www.sensesofcinema.com/2023/the-geometry-of-movement-computer-generated-imagery-in-film/from-legos-to-cgi-an-interview-with-kris-theorin/
- https://history.siggraph.org/?p=89547
- https://www.sidefx.com/tutorials/houdini-lego-explosion-based-on-pyro-sim/
- https://filmscalpel.com/lego-movie
- https://en.wikipedia.org/wiki/The_Lego_Movie
- /home/claude/rader-ai/clay-motion/research/07-motion-craft.md (sister doc, for [U] counts)
