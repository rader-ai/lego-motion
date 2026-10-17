# Brickification: translating ideas into brick design language (research, Oct 17, 2026)

Method: about 22 searches plus roughly 45 page fetches (designer and artist interviews, LEGO Masters judge interviews, BrickNerd, Brickset, Brothers Brick, LEGO Serious Play (LSP) guide and practitioner write-ups, the BrickGPT paper). Search budget ran out before I could cover data visualization in bricks, brick-world humor conventions and LEGO Certified Professional books directly; those parts are inferred and tagged [U]. Many interviews were thin on technique, so "what the source actually said" is separated from "my proposal" wherever possible. Source ids like [S4] are listed at the end.

## 1. Scale: pick it by the smallest thing the viewer must recognise
- Reference figure scales [S2]: Miniland 1:17, minidoll (Friends) 1:38, classic minifig 1:42 (4 cm tall), microfig 1:80, trophy fig 1:130, three-plate fig 1:150. The author calls figures smaller than about 1:125 impractical.
- The minifig is wider than a real person (about 1:25 by width) [S2]. Builders use minifig height to size buildings and minifig width to size vehicles [S3].
- LEGOLAND Miniland builders cite 1:20, a person about 10 bricks (30 plates) tall [S17]. This conflicts with the 1:17 figure above; treat the exact ratio as a studio convention to pick, not a fact.
- Studs per meter is not stated in any source I read. Computed from the standard 8 mm stud pitch [U]: 1:42 is about 0.34 m per stud (about 3 studs per m), 1:20 about 6 per m, 1:80 about 1.6 per m, 1:150 about 0.8 per m. Commenters on [S2] also claim about 3 studs per m at 1:42 (unverified).
- Micro scale means anything smaller than minifig scale; one 1x1 can be a person, and the micro figure sets the size of cars, doors, windows and trees [S3][S36].
- Detail that survives, by scale:
  - Life-size or sculpture scale: surface texture, shaded fur or feather color, expression. Needs hidden steel above about 1 m [S5][S20].
  - Miniland 1:20: observed, specific props (blankets, pretzels, hot dogs) and tiny personality gags (a briefcase popping open) [S17].
  - Minifig scale: story props, poses, printed faces; vignettes work here [S28]. One stud is about a third of a meter, so walls and floors read clearly [U].
  - Micro: one part stands for a whole object, details reduced to a few lines, and one house design repeated with small shape or color changes makes a town [S36]. In 400 to 1,600 piece landmark sets, details are inevitably lost [S13].
  - Mosaic: LEGO Art set 48x48 studs as the minimum for recognisability (2,304 round pieces) [S12]. Fine detail is lost; viewers step back to read it [S11].
- Scale is chosen for impact too: Sawaya's 80,020 piece T-rex is sized for awe [S43]. A vignette can be as small as 8x8, but then every detail counts [S28].
- Mixing scales: Miniland deliberately scales some elements up (18x, 25x) so visitors feel minifig-sized [S17]; micro can sit behind minifig scale as forced-perspective backdrop [S36]. Proposed rule [U]: one hero scale per frame, and any second scale needs a stated reason (background, joke, transition).
- Building and rendering must share one scale choice per scene file, or the "brick size" of parts will visibly disagree [U].

## 2. Silhouette and gesture first
- Tucker calls his work "abstract interpretations," keeps the essence and pure sculptural form, and says "I let your imagination fill in the details" [S15]. He works with what a brick wants to do; forcing it fails [S15].
- Kenney's process [S4][S5]: collect many reference photos, plan shape and size on graph paper or in a computer model, build several physical prototypes, view from every side, rebuild sections, then glue. Plan structure before building upward because you cannot carve or add to a surface.
- Kenney stylizes where the medium demands it: chunky bricks suggest a delicate beak and thin wings, while still aiming for "might come alive" realism [S5].
- Silhouette test [U, proposed]: fill the shape solid black at thumbnail size; if the subject is not identifiable, no detail will save it.
- Gesture [U]: no source defines it for bricks. Proposal: pick one line of action (lean, reach, dive), build the pose with hinges or ball joints, and let a slight asymmetry carry it (asymmetry draws the eye more than perfect symmetry [S10]).
- Toolkit named in The LEGO Builder's Handbook table of contents [S34] (contents only, chapter text not read): jumper-plate half-stud offsets, SNOT with headlight bricks, brackets and cascaded cheese slopes, hinge plates and turntables for angled walls, bending bricks, domes and spheres, STL based studs-up and studs-out sculptures.
- SNOT math example [S13]: five plates equal the height of a 1x2 plate on its side; vary color between levels to suggest depth and overhangs.
- Curves: round forms from square bricks are the main challenge; Miniland builders use graph paper, software and visual judgment, and repeating a curve across a scene helps it feel real [S17]. A real example of a part standing in for curves: car tyres for the circles on the Library of Birmingham model [S18].
- Geometry facts [S33]: parts follow fractions of brick height, which is why odd orientations fit; building is additive, never carved. Clone bricks vary in wall thickness and make tall or wide builds bend; Sawaya wishes for longer bars (1x5, 1x7) to bridge warp [S33][S44].
- System vs sculptural (my decision rule [U]): system building (grid aligned, studs up, brick and plate, modular facades) for architecture, interiors, minifig vignettes and mosaics; sculptural building (SNOT, angles, hinges, slopes) for animals, people, vehicles and anything organic. Judges say builders should know whether their strength is sculpting or minifig storytelling [S8]. Sawaya likes sculptural elements even inside sets [S1].
- Large works: Bright Bricks pipeline is mesh, brick conversion, hollowed 2-stud shell, steel skeleton, then layer-by-layer build [S20]. BrickGPT is a voxel version: 20x20x20 grid, 8 plain brick types, physics checked [S40][S41].

## 3. Color reduction
- LEGO has roughly 40 commonly available colors versus 16.7 million digital colors, so subtle gradients do not translate; bold shapes with few colors (pop art) do [S11]. Miniland builders work with 60+ colors and 17,000+ elements [S17]; Bright Bricks paints from 32 colors and blends or speckles to fake extra shades like fur [S20].
- Color is fixed and cannot be mixed like paint, which makes copying paintings hard [S43]. Part availability by color is a real constraint: check that slopes, brackets and SNOT parts exist in the colors chosen [S9]. Kenney's darker pink exists only in tiny sizes, which forced a dense small-brick build [S5].
- Mapping method for images [S11]: nearest color in RGB is fast but crude; Lab space with CIEDE2000 matches perception better. Dithering reduces blotchiness and helps skin tones but adds grain. Brighten dark images, raise contrast and saturation slightly.
- Limiting a palette [S9][S10]: start from a harmony (monochrome, complementary, analogous, triadic, split-complementary); use two or three hues plus black, white or gray; assign colors by function (one for utility parts, another for engine parts).
- Value contrast [S9]: balance light colors with one darker grounding color; keep the backdrop muted so it does not compete. Light bluish gray is a safe neutral canvas [S10].
- Accents [S9][S10]: a small amount of high-saturation color on a muted body gives pop; too many highlighted areas read as chaos; an even distribution of colors is not necessarily best, and offsetting a stripe from the middle helps.
- Color blocking as a named technique is not covered by BrickNerd [S9]; for greebling they suggest solid blocks with one or two greeble colors and gaps for the eye to rest [S10].
- Color follows story [S9]: ask what the build should feel like and choose colors that reinforce it (dark green and gold forest queen; vibrant colors on a gray frame for cyberpunk). Sawaya picks gray for melancholy [S44].
- Realism hacks: several greens for foliage, coral as a terracotta stand-in (commenter claims) [S37]. Shades like dark red, dark blue, dark green and reddish brown vary between sets in practice (commenter claims, [S37]).
- Architecture precedents: LEGO Architecture Studio used only white and transparent to remove color as a distraction [S36]; micro landmark sets use dark gray bands between floors, black tile borders and a few accent colors on mostly white [S13]; early Architecture sets used fewer colors to welcome non-fans [S14].

## 4. Iconic simplification and caricature
- Official rule from a LEGO Architecture designer: capture the core, keep it recognisable at any scale ("instantly recognisable, no matter the scale"), and compromise on detail as piece counts shrink [S14]. The same building is designed at several price points, each keeping different aspects [S14].
- More detail can reduce accuracy: the White House set with more detail is more out of proportion than the simpler older one [S13]. Detail budget should follow the part budget.
- Seed piece idea [S13]: pick one key part (a printed clock) and let it anchor the whole design. Berard on Big Ben: use bricks instead of plates where possible, alternate repetitive sections, and hunt for the part that already resembles a feature (a tan tooth piece for the facade shields) [S16].
- Part-as-metaphor [S35]: judge a part by shape, color and connection points, not its name; one minifig hand is a hand, a row of hands is a facade pattern; mixing categories (food plus Technic pins) often works best.
- Recognition proxies for caricature [U, proposed]: one-second read test = show the render for one second to a person (or a vision model with a "name this" prompt) and check they name the subject; fail means exaggerate the silhouette, a signature color, or one signature prop, and delete everything else.
- Exaggeration conventions in brick language [U]: oversize the identifying feature (sign, hat, tool), flatten faces to a few prints, round chunky forms, and use one saturated accent. Kenney's chunky-for-delicate trade is the sourced case [S5].
- Complex subjects get easier to grasp in bricks: Titmarsh's cutaway jet engine shows what each part does, which a real engine cannot [S21].
- Faces are the hardest simplification: Kenney reports many rebuilds because tiny changes shift expression [S4].

## 5. Storytelling inside a build
- Judges: story is one of three core criteria with technical skill and creativity; technical builds can still feel dull; polished story that pulls viewers into a world wins; "make us smile" is the goal [S6][S7][S8].
- Vignette rules [S28]: plan the narrative first; choose whether people or environment is the subject; pick one viewing angle like a 2D image; give each character a motive (what are they doing, where going, why); every element supports the story; park good ideas that do not.
- Photograph or render as the final product [S29]: closer cinematic frames, negative space for tension, subject off-center, diagonals leading to the focal point, left-to-right progression from empty to full, capture a "pregnant moment" and do not over-explain. Non-LEGO elements can add realism; all-LEGO scenes with unrealistic colors or blocky perspective are less believable [S29].
- Miniland practice [S17][S19]: realism comes from observed details; people get small personality details (briefcase, fancy hats at the races); crowds can be added after the stage is built; geography is collapsed and landmarks are moved or invented ("Brick Park"); humor is allowed, including a figure of a public official insulting a reporter [S19].
- Humor and Easter eggs: LEGO Movie in-jokes include Benny's broken helmet chin strap, a common defect of 1980s space sets [S30]. A wider catalog of brick-world humor conventions (puns on parts, minifig cameos) was not researched [U].
- Film look rules [S30]: CGI made to look like stop motion; rigs follow the limits of real minifig articulation; sets built in LEGO Digital Designer so bricks connect as real ones do; minifigs photographed under microscopes to capture seams and grime. Rule for the studio [U]: never let anything move in a way a real assembly could not.
- "Build anything" vs instructions [S30][S31][S32]: Master Builders create without manuals and the villain suppresses them; the ordinary Emmet becomes "the Special"; the ending has a boy and his father, where the father learns to stop suppressing his son's creativity and play. Commentary notes the tension in LEGO itself: sold as open bricks and as instruction sets, and the freedom is bounded by a corporation's parts [S31]. Studio use [U]: two looks, "set" (clean, canonical, instruction-ready) and "MOC" (improvised, personal, imperfect), chosen by whether the idea is a product or a memory.

## 6. Abstract and non-physical ideas
- LSP is the best-documented method [S22]. Cycle: connect, create, reflect, extend. Each unit: challenge, build, share. The builder alone decides what the model means; the facilitator does not interpret. Skill-building goes creature, then "ideal or worst boss" in four minutes, then "My Monday mornings."
- Meaning follows making: bricks are low-resolution, so models become symbolic rather than literal, and people attach abstract meaning to them [S27]. Follow-up questions to read a model: what depends on something else, what is hidden [S27].
- Documented examples: a steep ladder for an uncertain workload [S24]; a black base plate with colored bricks above for layers of competency and learning, plus a minifig trapped turning cogs with a springboard out of a window for workload and possibility [S25]; one white brick as a polar bear, red and green bricks as stop and go, or as two work groups [S26]; a customer journey model with spots for frustration and delight [S27]; identity models of who you are on a team [S22].
- LSP gaps [S22]: the guide gives no specific challenge for risk, and emotions are handled by externalizing them as objects.
- Studio translation [U]: LSP has a builder who explains the model. A render has no explainer, so it needs visual grammar the viewer can decode: spatial metaphors (up is growth, base is foundation, gap is risk, bridge is connection), one object per idea, color by role, and an optional one-line caption.
- Candidate mappings [U]: success is height or a flag on top; risk is a narrow gap, a loose stud or a lone tall tower on a small base; teams are figures sharing one connected baseplate; workflow is a track or conveyor with ordered stations; data is bars or stacked columns (mosaic pixels at tiny sizes); software is machinery or architecture with floors; emotion is color plus posture (Sawaya's Yellow opens a figure's chest and spills yellow bricks [S43]).
- Brand builds are cutaways and symbols: Titmarsh built a cutaway jet engine, a Tube map of 1,000+ bricks over four days, and a 12.2 m tree with steel frame [S21]; Bright Bricks makes a McLaren and a Tower Bridge replica for brands, via mesh, hollow shell and steel [S20].
- Brand and trademark caution [U]: LEGO Ideas bars direct recreation of brand logos or mascots (small details are fine) [S39]; Sean Kenney's site carries a trademark notice that LEGO does not sponsor it [S46]. Brick Studio should say "brick-built style" and avoid the LEGO name and logo; needs legal review.

## 7. People and places
- Likeness is hard: Kenney reports many rebuilds for faces [S4]; the LEGO Ideas guidelines address photos of real people in submission images but not brick depictions of individuals [S39]. A Miniland figure has parodied a real public official, so likeness is a policy choice, not a technical limit [S19].
- Generic vs specific [U]: generic person = role cues (hat, apron, headset, tool), one signature color block, no face prints beyond a simple smile; specific person = one or two chosen cues the owner approves (hair color block, glasses, jacket color, one prop). Never claim a likeness; name it in the caption instead.
- Few figures beat crowds: Ideas guidance prefers a handful of well-chosen minifigs [S39]; crowds are added last in Miniland [S17].
- Places: visit and observe, because photos miss details [S17]; Birmingham Miniland used photos and CAD for 45 landmarks, with a 1.73 m BT Tower of 4,700 pieces and 2,500 minifigs planned [S18]. Distances are collapsed and landmarks shifted [S19].
- Workplace or city proposal [U]: 3 to 5 landmark silhouettes plus signature color and one tiny local gag; micro for a city, minifig scale for a workplace.
- Keep builds current: Miniland refreshes models as the real place changes [S19].

## 8. Failure checklist (amateur and AI brickification)
- [ ] Too literal: copied every window and logo instead of the silhouette; detail budget exceeds part budget [S13][S14].
- [ ] Wrong scale mixing: mixed figure and building ratios without a device; parts of different "brick size" in one frame [S2][S3].
- [ ] Color soup: more than about 5 hues, even distribution, no grounding dark, no accent [S9][S10].
- [ ] Impossible palette: colors or shapes that do not exist as parts (a color-part pairing, a smooth curve with no slope) [S9][S37].
- [ ] Part salad: random exotic parts without a seed idea or repetition logic [S13][S35].
- [ ] No story: no focal point, no motive, no moment [S28][S29].
- [ ] No read: fails the one-second test, silhouette mushy [U].
- [ ] Off-model hands and props: AI images of minifigs got hands and props wrong and drew fan criticism [S42].
- [ ] Unbuildable: floating bricks, no clutch, unsupported spans; BrickGPT needed rejection sampling and physics rollback (valid/stable 37.2%/12.8% without them, 100%/98.8% with) [S40].
- [ ] Blocky approximations with no art direction: BrickGPT output is blocky and needs human creativity [S41].
- [ ] Stud grid ignored: uneven stud spacing, wrong stud count on a wall, flat surfaces with no studs or tiles chosen on purpose [U].
- [ ] Mosaic mistakes: too dark, too detailed, wrong aspect ratio [S11].
- [ ] Background fights the hero: busy base color near the subject [S9].
- [ ] Over-explaining: captions or labels that replace visual metaphor [S29].
- [ ] Stylistic drift: smooth CGI look instead of real plastic seams, scuffs and joins [S30].

## 9. The method (repeatable, with checks)
1. Brief: write the idea as one sentence plus audience plus mood. Check: a stranger can paraphrase it.
2. Pick the question the image must answer (what is it, who is in it, what is happening). This sets the smallest thing to recognise and so the scale (section 1).
3. Choose scale and mode: micro, minifig, Miniland, sculpture or mosaic; system or sculptural. Check: one hero scale.
4. Silhouette pass: block shapes in 2 to 3 flat values. Check: black-fill thumbnail still names the subject.
5. Palette: 3 main hues max, one grounding dark, one accent, a muted backdrop. Check: each color exists in the target parts and brick colors.
6. Seed and parts: choose one seed part, a repeatable module, and 5 to 10 hero parts (slopes, SNOT, hinges) for curves and details. Check: every part has a plausible attachment.
7. Story beat: one character or one moment, with motive; place the focal point on a diagonal or off-center. Check: you can say what happened a second ago.
8. Detail budget: add only details that serve the read or the story; add one local gag. Check: delete one detail and see whether the read improves.
9. Physics pass: clutch, support, steel for big builds; no impossible joins. Check: stability rule from the BrickGPT approach [S40] or a real build.
10. Read test: one-second glance test, then a thumbnail test, then a "what is this about" test with someone who has not seen the brief. If it fails, simplify, do not add.

### Worked examples (all design proposals [U], parts generic)
- Coffee shop. Scale: minifig, one corner bay of a modular facade. Palette: dark brown and tan base, white trim, one orange or teal awning accent. Key parts: awning from slopes or tiles, large window frame, espresso machine from a gray tile with clips and silver plates, a barista minifig with a printed apron, steam from a translucent round plate. Story: first customer of the day, mug being handed over. One-second read: awning plus cup sign plus steam.
- Software product team. Scale: minifig, a vignette about 16x16 studs. Palette: light gray and white room, one brand blue, a yellow accent only on the shipped feature. Key parts: four figures around a board of 1x1 round plates in three columns (to do, doing, done), a hinged laptop, a red 1x1 bug stuck in the "doing" column. Story: someone moves a plate to "done" while another squashes the bug. Read: people moving tickets across a board.
- Soccer game. Scale: micro, 48x48 pitch with 1x1 round tile players. Palette: two greens in alternating stripes (value contrast), two high-contrast kits (red vs blue), white ball, crowd as warm confetti behind. Key parts: bar or arch goal frames, round plates for players, a stand of stepped bricks. Story: striker mid-kick, keeper diving, ball in flight on a clear bar. Read: striped pitch plus goal plus ball in the air.
- Customer feedback. Scale: minifig vignette with sculptural elements. Palette: warm orange customer side, cool blue product side, translucent yellow speech bubbles as the signal. Key parts: a customer minifig speaking into a funnel or mailbox, small translucent round plates as bubbles traveling along a track, a workshop table where a product block changes from rough gray to polished blue. Story: the block improves after the loop closes. Read: people talk, a thing gets better, and the arrow points left to right.

## Gaps and open questions
- No source defines the one-second read test or the "gesture" concept for bricks; both are proposals. Studs per meter was calculated, not found. The Miniland ratio is 1:17 or 1:20 depending on source.
- Not covered: data visualization with bricks, humor conventions at depth, Sean Kenney's and Sawaya's books, Brothers Brick design-thinking essays beyond tags, LSP risk and success models. Search budget ran out.
- Open: how far to push "set" vs "MOC" looks in the pipeline; whether the studio may depict real people; how to enforce physical plausibility in renders.

## Sources
[S1] https://www.brothers-brick.com/2020/01/20/building-lego-masters-exclusive-interview-with-brick-artist-nathan-sawaya-news
[S2] https://brickarchitect.com/scale/
[S3] https://www.brothers-brick.com/2014/12/29/ask-a-lemur-minifigs-conventions-and-scale
[S4] https://theimagineteam.com/post/an-interview-with-sean-kenney-artist-who-works-with-lego-bricks
[S5] https://ncarboretum.org/2016/07/27/little-legos-larger-life-interview-artist-sean-kenney/
[S6] https://www.brothers-brick.com/2020/02/05/everything-you-want-to-know-about-lego-masters-judges-amy-corbett-and-jamie-berard-feature/  [S7] https://www.brothers-brick.com/2020/01/14/judging-lego-masters-exclusive-interview-with-lego-designers-jamie-berard-and-amy-corbett-news
[S8] https://brickset.com/article/48353/interview-with-lego-masters-judges-brick-master-amy-and-brick-master-jamie
[S9] https://bricknerd.com/home/how-to-use-color-in-lego-builds-5-6-25  [S10] https://bricknerd.com/home/using-color-theory-and-composition-in-lego-mocs-3-25-24
[S11] https://bricknerd.com/home/everything-you-want-to-know-about-lego-mosaics-11-12-24
[S12] https://brickset.com/article/52275/interview-with-samuel-johnson-lego-art-designer
[S13] https://brickset.com/article/53073/micro-scale-architecture
[S14] https://brickset.com/article/29331/interview-with-architecture-designer-lars-(2)
[S15] https://www.thehenryford.org/explore/blog/an-interview-with-adam-reed-tucker
[S16] https://zusammengebaut.com/pushing-the-limits-interview-with-lego-designer-jamie-berard-about-big-ben-10942/
[S17] https://brickarchitect.com/2025/interview-legoland-california-with-master-model-builder-pj-catalano/
[S18] https://www.brickfanatics.com/building-birmingham-landmarks-for-legoland-discovery-centre/
[S19] https://strangeharvest.com/legoland/
[S20] https://develop3d.com/profiles/brick-by-brick-bright-bricks-lego-cad-solid-edge-st10-design/
[S21] https://old.designcurial.com/news/duncan-titmarsh-4141657/
[S22] https://www.lego.com/cdn/cs/set/assets/blt8ec1d6ff766ddfd4/LEGO_SERIOUS_PLAY_OpenSource_14mb.pdf
[S24] https://blogs.sussex.ac.uk/business-school-teaching/2019/03/26/my-experience-using-lego-serious-play-to-teach-human-resources-organisational-behaviour/  [S25] https://blogs.ed.ac.uk/teaching-matters/?p=17506
[S26] https://discoverysession.substack.com/p/one-hour-that-will-convince-you-of  [S27] https://mmacfadden.substack.com/p/making-before-meaning
[S28] https://www.brickfanatics.com/lego-dinosaurs-savage-lands-the-art-of-the-vignette
[S29] https://www.brothers-brick.com/2020/06/03/picturing-lego-using-lego-bricks-to-tell-a-story-through-photography-feature
[S30] https://en.wikipedia.org/wiki/The_Lego_Movie
[S31] https://tropicsofmeta.com/2014/02/10/the-lego-movie-and-the-gospel-of-the-creative-class/  [S32] https://thetoyinsider.com/the-lego-movie-builds-on-individuality/
[S33] https://aestheticsforbirds.com/?p=877
[S34] https://www.oreilly.com/library/view/-/9781098182267/
[S35] https://www.toypro.com/gb/news/1431/that-piece-doesnt-belong-there-does-it-surprising-use-of-parts-in-lego-mocs
[S36] https://thebrickblogger.com/2013/03/lego-micro-building-tutorials/ and https://thebrickblogger.com/2013/10/more-lego-micro-building-tutorials
[S37] https://thebrickblogger.com/2020/07/working-with-the-lego-color-palette/
[S38] https://oreilly.com/library/view/the-unofficial-lego/9781593270544/ch06.html
[S39] https://ideas.lego.com/guidelines
[S40] https://arxiv.org/html/2505.05469v3  [S41] https://www.theregister.com/2025/05/12/legogpt/
[S42] https://www.brickfanatics.com/why-lego-fans-critical-lego-ninjago-ai-images
[S43] https://artpublikamag.com/post/pop-culture-internal-angst-and-the-lego-art-of-nathan-sawaya-interview-with-the-brick-artist  [S44] https://brickset.com/article/48436/interview-with-nathan-sawaya-lego-artist-and-lego-masters-consulting-producer
[S46] https://www.seankenney.com/commissions/
