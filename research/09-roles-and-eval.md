# Brick studio roles and evaluation (research, Oct 17, 2026)

Method: about 19 web searches (the shared search budget ran out mid-run) and about 40 page fetches, favoring Animal Logic papers, LEGO.com pages and postings, arXiv, GitHub, Hugging Face and LDraw.org. [U] = unverified or inferred. Many fetch results were machine-condensed, so items tagged (secondary) deserve a second look before anyone relies on them.
Gaps: Flickr CC groups, archive.org brickfilms and most Wikimedia Commons category counts were not verified. The Nature paper for the Gdansk brick image dataset, a Kaggle brick dataset, Brickognize and three LEGO careers pages hit HTTP 429 and were not read. No IMDb crew lists were pulled and no LEGO box art or marketing source was found. Clay-studio content (director/producer split, hard gates plus blinded VLM pairs, own-shoot gold) is assumed and not repeated.

## 1. Departments and roles on the brick features (Animal Logic)
- Animal Logic made The LEGO Movie (2014), The LEGO Batman Movie (2017), The LEGO Ninjago Movie (2017) and The LEGO Movie 2 (2019). TLM2 had 316 crew at the Vancouver studio.
- Named leads, film 1: production designer Grant Freckelton, head of animation Rob Coleman, CG supervisors Damien Gray and Aidan Sarsfield, lighting tech lead Max Liani, lighting supervisor Craig Welsh, cinematographer Pablo Plaisted. TLM2: production designer Patrick Hanenberger, art director Kristen Anderson, animation supervisor Dave Burgess, layout supervisor Regis Schuller, CG supervisors John Rix and Emmanuel Blasset, FX supervisor Mark Theriault. Ninjago FX supervisor: Miles Green.
- No "brick supervisor" or "brick department" title appears in any source read. Brick work was spread across: art department (LDD mockups, color and design direction), asset/modelling/surfacing, Assembly and Layout (Assembly artists are named in the Millions of Bricks paper), pipeline R&D (ALF rigging, Brick-Tools, ShellBake), animation, FX (Houdini) and lighting. A single library owner is inferred, not documented [U].
- The brick library was the shared contract. LEGO supplied a parts database with size, weight, center of gravity, connection points and a joint chain for flexible parts (CAD per piece in a secondary source). Animal Logic remodelled 2000+ bricks with subdivision surfaces. ShellBake turned LDD files into several detail levels and formats (render, GL, FX point clouds) tagged with brick database indices. Bricks were looked up by ID with sidecar shader data.
- Departments mostly talked through files and tools. LDD model to shells; Layout wrote a text config per environment; a shot database assembled shots; animators got GL display shells; scene partitions auto checked in as environments changed. Brickville (secondary) let artists build environments live from the library. For TLM2, a LEGO Design Manager in Billund (Mauricio Bedolla) coordinated all movie asset creation with the Warner and Animal Logic design teams.
- What fed the look: bricks at every desk, trips to Legoland California and a tech scout at Legoland Billund, minifigs under microscopes for seam lines and grime, weathered outdoor models, and a consult with photographer Vesa Lehtimaki (Avanaut). The directors' touchstone was stop-motion (Robot Chicken, secondary). Chris McKay (animation director on film 1) and Trisha Gum (co-director on TLM2) both have stop-motion backgrounds.
- Approval loops are thin in the record. The cinematographer blocked shots, consulted the directors and iterated. The art director alone decided when any camera blur was used. Directors reacted to brick-water tests. On the LEGO side, design VP Matthew Ashton was an executive producer on film 1 and the Denmark team was one of three production sites. LEGO closely watches color IDs and palettes in films: Ninjago burn effects had to snap to the nearest legal color. No formal LEGO sign-off steps were documented.
- Animator rules: no squash and stretch and no deforming shoulders; move only where the toy articulates (neck, arms, wrists, waist, legs; not elbows or knees, secondary); characters stay planted on studs; no motion blur, replaced by "brick blur" (a smear of color-matched bricks); shot on twos and threes; background characters re-conformed from bricks.
- Brick FX. Film 1 rebuilt smoke, fire and water as bricks (voxel sims replaced by bricks of matching size) and demolished buildings with glue constraints generated from brick connectivity, so they broke along real joins. TLM2 hand-sculpted explosions in LDD and swapped them procedurally per frame, aiming for all-LEGO but keeping a little real dust and smoke. Ninjago ran real-world style sims, then subdivided and jittered bricks to show burning.
- Scale anchors. TLM2: 3,433 unique brick types, 523 unique characters, Metalbeard 251 bricks, biggest shot 161,737,777 bricks, 6,600 animation days. Film 1: 2000+ bricks modelled (Animal Logic paper). Animation Magazine cites 3,863,484 "unique bricks" and 15 million total, which conflicts and likely counts instances [U].
- Brickfilm teams: no documented role split found. Wikipedia says amateur crews, 12, 15 or 24 fps, tools Stop Motion Studio, Boats Animator, Eagle Animation, Dragonframe. Hubs: Brickfilms.com (2000), Bricks in Motion (2001; THAC and BRAWL contests), Brickfilm Day (2018).

## 2. LEGO Group roles (what each owns, how "good" is judged)
- Set/product designer: works to a brief, builds concept models and sketches, pitches verbally and visually, inside a design team of about 600 in Billund (Technic model designer posting). Hiring asks for builds made for a target audience and knowledge of child development. Judged on brief fit, pitch, feedback and deadlines.
- Real set path (Colosseum article): senior designer builds a one-brick-thick concept, passes an Internal Selection Event, a second designer develops it, second selection event, one month to estimate production cost, four partial models, oven heat test to simulate weathering, building instruction expert orders the build, final model quality meeting. First talk Feb 2019, quality review about Aug 2019, release Nov 2020.
- Judged on: concept appeal, cost, durability, stability (an oval base was added), buildability. Piece count was not a headline criterion; it rose when bricks were swapped for larger ones to cut cost.
- Model coach and prototype builder: a model coach appears in the Colosseum build. Prototype builders turn sketches or digital concepts into physical prototypes at varied fidelity, finish them for play tests and push inventors on manufacturability (internship posting, not a full job spec).
- Element designer (Karsten Bunch interview): team of about 50. Owns the whole family of parts around a request. Stages: planning, exploration, selection, concept, design for manufacturing, design freeze (engineers take over), independent lab tests and approvals. Judged on fit with existing elements, manufacturability, function specs (launch force capped in joules), impact and safety tests, kid play tests and model designer feedback.
- Graphic designer and decoration: the article does not define the role but shows the constraint. Designers get a limited yearly number of "frames"; a new color for a part, or a new or returned part, costs one frame. A digital brick library tracks pending recolor and new-part requests, teams share frames, and prints are preferred on small or placement-sensitive parts. Takeaway: a part existing in a color is a managed, scarce fact.
- Building instruction specialist (BrickLink Designer Program posting): crafts, develops and tests instructions end to end, builds physical and digital models to set build flow, tests fan-designed models. Tools: BrickLink Studio and InDesign. Judged on a natural build flow for the age group's motor and cognitive skills, with QA in every task.
- Box art and marketing imagery, and LEGO's own animation roles: no primary source retrieved [U].

## 3. What differs from a clay studio
- Drop: sculptor, mould and armature makers, costume makers, replacement-face prototyping, clay surface sign-off. Faces become a choice among printed face parts; a custom print is a new asset needing a decal author [U].
- Keep unchanged: Director/Producer split, weekly shot review, animation supervisor, DP and lighting, VFX supervisor, editor, sound.
- Merge: production designer, set designer and model builder into one Build Designer who owns each scene model as a parts-level file; palette authority sits with the same owner or the Librarian.
- Replace the fabrication supervisor with a Brick Librarian: owns the part catalog and ID map (LDraw, Rebrickable, BrickLink), color availability per part, shells and detail levels, bevel and jitter settings, and the "this part exists in this color" gate. Closest Animal Logic analogue is the library plus tools team [U].
- Add: Build Validator (code gate, section 6); Build-Order or Instruction designer (needed for instruction-style outputs and construction animation, which Animal Logic treated as its own workflow); Brick FX (water, fire, smoke and demolition as bricks, driven by connectivity); Decoration designer (prints, stickers); Palette and IP compliance.
- Failure modes shift from boil, fingerprints and sagging to structural impossibility (floating, fused or interpenetrating bricks, illegal connections, unavailable colors) and over-perfect CG plastic.
- Communication contract: ID-keyed library, connectivity data, text configs (the Animal Logic pattern), so agents hand files to each other, not prose.
- Cadence: clay's twos at 24 fps carries over. Film 1 used twos and threes.
- IP: LEGO's Fair Play page covers non-commercial fan sites, never allows the LEGO logo, bars LEGO in domain names, and treats copying instructions and catalog or package photos as infringement. It is silent on film and AI. Studio name and branding should avoid LEGO marks; run commercial plans past counsel (not legal advice).

## 4. Openly licensed reference material
- Wikimedia Commons, verified: Category:Lego vehicles has 17 subcategories and 47 files (seen on a Commons mirror), including Lego models of trains (70 files), loaders (6), cranes (4), tractors (3), forklifts (3), buses (2). Category:Lego minifigures has 3 subcategories and 31 files.
- Category:Lego, Lego models, Lego sets, Lego bricks, Lego Technic, Lego Star Wars and Brickfilms could not be fetched (not found or rate limited). That is not proof they are absent; counts remain open [U].
- Commons licenses vary per file (CC BY, CC BY-SA, CC0, public domain) and must be recorded per file. A CC license on a photo covers the photographer's rights, not LEGO's trademarks or set designs [U, not legal advice].
- Flickr Creative Commons and archive.org: not verified. Use Flickr's license filter through its API and log per-photo license, author and URL [U]. Wikipedia's Brickfilm article names no CC or public-domain brickfilms, so openly licensed brick animation is probably scarce.
- Earliest brickfilms: a German LEGO ad from about 1960 (displayed at LEGO House) and En rejse til manen (1973, uploaded 2013). Their licenses are unknown.
- LDraw Official Model Repository (library.ldraw.org/omr): official sets as LDraw files, useful as known-valid builds more than as photos. The page states no license and no model count. Spec revised Dec 17, 2020, license update Nov 22, 2023.
- LDraw Parts Library: CC BY 2.0. The official library has 17,164 unique shapes or patterned parts (Sept 2026 update added 121 parts). LDraw.org says renders made from the library are not derivative works and it will not enforce copyright on them; credit is requested for commercial rendered instructions.
- No CC0 brick photo set was found this session [U]. Safest gold: our own bricks, filmed and photographed by Chris, plus renders of the same builds. Official LEGO stills stay out of judge APIs, as with studio stills in the clay GOLD_SET.

## 5. Known-bad tells for brick renders and animation
Sourced from what Animal Logic added to avoid them:
- Too perfect: razor edges with no bevel, bricks aligned to the pixel with no angle jitter, uniform clean plastic, no seam lines, no wear, dust, grime or oxidation, no fine print or decal detail. Animal Logic added bevels, jitter, per-brick wear and decals, seams and grime.
- Motion: stretchy or bendy limbs, elbow or knee bends on a standard minifig, deforming shoulders, smooth motion blur, feet not planted on studs, every frame a new pose.
- Effects: fire, smoke or water as smooth volumetric or particle sims rather than bricks (film 1 and 2 used bricks).
- Color: off-palette colors, gradients, banding or painted-on texture; burnt bricks should snap to a legal color.
- Depth: DOF not matched to real lenses at miniature scale.
Inferred from how image generators fail on bricks [U]:
- Wrong or missing studs, mismatched stud counts, studs not round or inconsistent in size, clay-like blobs that only look "LEGO", gibberish print on faces and torsos, impossible parts, bricks floating or fused or overlapping, 1x2 and 2x4 proportions off, slopes and curves that do not exist as parts, hands that cannot grip, inconsistent minifig proportions between frames.
- Identity drift of a build between frames (brick count or layout changes), and brick-for-brick pop when a shot cuts.

## 6. Automated checks unique to bricks, and code that exists
- Stud-grid placement. BrickGPT stores each brick as height x width at (x,y,z) on a 20x20x20 grid, bottom to top. LDraw works in integer units (stud pitch 20, brick height 24, plate 8; [U], well known but not re-checked here). Check that axis-aligned parts land on lattice multiples and rotations are multiples of 90 degrees. Write a small parser; LDraw is plain text.
- No interpenetration. BrickGPT rejects any brick whose voxels overlap an earlier brick. General parts (slopes, Technic, minifigs) need a mesh-level test that tolerates the legitimate stud-in-tube overlap [U].
- Rigid parts never deform. Per frame, each part's transform must stay rotation plus translation (no scale, shear, shape keys or armature modifiers). Animal Logic's metadata marks flexible parts with joint chains; whitelist those from the catalog [U check design].
- Valid color per part. Rebrickable API /lego/parts/{part_num}/colors/ returns colors within sets (fixed Jul 2020). Limits: about 1 request per second, page size to 1000, batch with part_nums, bans possible on abuse. Rebrickable CSV downloads are free for any use with a mention requested; the cached downloads page showed only four tables dated Jan 2017, so confirm current tables (elements.csv) yourself [U]. BrickLink is the other source; its API terms were not checked.
- Connection and stability. BrickGPT solves a nonlinear program (Gurobi) minimizing net force and torque with friction limits. A structure is stable only if every brick scores above zero. Under 200 bricks it takes about 0.35 s on average. Its rollback raised stable output from 24.0% to 98.8%. Gurobi is free for academics only; the repo has a less accurate connectivity-only mode (--use_gurobi False). Animal Logic generated glue constraints from connectivity for Houdini rigid-body sims, a second reference design. An open solver rebuild is plausible [U].
- Joint limits for figures. Source rule: articulate at neck, arms, wrists, waist and legs, not elbows or knees (secondary). Actual angle ranges are not sourced. Measure on Chris's physical figures and store a limit table; check per frame [U].
- Piece counts. Count parts in the LDraw file and compare to a scene budget. Rebrickable sets include piece counts for sanity ranges (old CSV shows num pieces). Animal Logic anchors above.
- Palette compliance. Check assigned color IDs against the allowed list (LDraw color table or Rebrickable colors). A pixel-level check after lighting needs tolerance [U].
- Rendering stack. ImportLDraw (GPLv2 or later; Blender 4.0 to at least 5.1.0; bevel, gap, instancing, Principled BSDF materials; Cycles photoreal look or EEVEE instruction look). It has no validation feature. BrickGPT uses it as a submodule.
- Why code, not VLMs: LEGO-Puzzles (29 MLLMs tested, 1,100 elementary questions) found the best at least 20% behind humans, and step planning accuracy falling to 0% as steps grow. Judge panels should not be trusted on buildability.

## 7. Datasets (licenses)
- StableText2Brick (Hugging Face AvaLovelace/StableText2Brick): MIT. 47,389 rows (train 42.6k, test 4.79k), 44.3 MB parquet, 5 captions each, per-voxel stability scores. Limits: 1-unit-tall cuboids only, 20^3 grid, 21 ShapeNet categories. The objects come from ShapeNetCore; the ShapeNet terms may restrict commercial use despite the MIT label [U].
- BrickGPT code and model: MIT, fine-tuned from gated Llama-3.2-1B-Instruct (Llama terms apply [U]).
- Gdansk photos and renders: about 155,000 photos and nearly 1.5 million renders of single bricks labeled by brick ID and catalog category (DOI 10.1038/s41597-023-02682-2, likely Scientific Data). License and class count not read [U]. Part-level only, not scenes.
- LEGO-Puzzles benchmark (arXiv 2503.19990): 1,100 elementary questions plus a planning set; license not stated [U]. Use it to test which judge models can reason about brick geometry at all.
- MEPNet (arXiv 2207.12572): three LEGO manual datasets; release not confirmed [U].
- LDraw parts, OMR and Rebrickable CSVs as covered in sections 4 and 6.
- Not retrieved: a Kaggle brick image set and Brickognize recognition API [U].

## 8. Suggested deltas to the clay docs
- Gold shoot (bricks): 1 to 2 hours with real bricks and two or three minifigs, locked camera, one hard key, grey card, color checker. Clips on twos: turn and blink with swapped faces, walk on studs, bricks placed one by one, a brick-built splash. Macro stills of studs, seams, edge wear, fingerprints.
- Unique advantage: build each gold scene as an LDraw file first, so every photo has a ground-truth model. Render that model in Blender and judge render against photo.
- Known-bad controls: remove bevels, remove jitter, add motion blur, bend limbs, float or overlap a brick, drop studs, off-palette colors, plus lazy-prompt "LEGO style" outputs from 3 or more generators.
- Layer A adds: grid, overlap, connectivity and stability, rigid-transform, part-color, palette, joint-limit, foot-on-stud contact, brick-count stability across frames. Drop clay boil and ghost-blend detectors unless the render pipeline produces them.
- Layer B dimensions: plastic surface and edge wear, stud and part geometry, miniature lighting and depth, scale and DOF, minifig fidelity, handmade cues. Buildability stays out of the panel.

## 9. Open questions
- Does any source name a brick library owner or a LEGO approval gate? Not found; the IMDb crew lists for films 1 and 2 would show it.
- Real counts and licenses for Commons LEGO categories, Flickr CC groups, archive.org brickfilms: not verified.
- Joint angle limits, current Rebrickable part-color tables, ShapeNet terms for StableText2Brick, and the Gdansk dataset license need checking.
- How LEGO's policies treat AI-made brick films for commercial use: no source found.

## Sources
https://www.fxguide.com/fxfeatured/brick-by-brick-how-animal-logic-crafted-the-lego-movie/
https://animallogic.com/wp-content/uploads/2023/07/The-LEGO-Movie-Construction-Animation-And-Demolition.pdf
https://animallogic.com/wp-content/uploads/2023/06/Millions-of-Bricks-Rigging-Animation-Render.pdf
https://animallogic.com/wp-content/uploads/AL_TLM2_Backgrounder.pdf
https://vfxvoice.com/the-building-blocks-behind-the-lego-movie-2/
https://www.brickfanatics.com/the-lego-movie-2-when-movie-makers-and-toy-designers-collaborate
https://www.flickeringmyth.com/bricklayers-making-of-lego-movie
https://www.creativebloq.com/3d/legomoviefeature
https://animationmagazine.net/?p=180487
https://cartoonbrew.com/feature-film/animal-logics-rd-lego-ninjago-movie-included-going-beach-153436.html
https://jobbank.dk/job/2603741/lego-koncernen/lego-technic-tm-model-designer-lego-design
https://legocareers.dejobs.org/irvine-ca/building-instruction-specialist/FFE7760BB6B2499FA2E479500D403270/job
https://www.lego.com/en-us/careers/job/prototypes-internship-8a9bbb4798331001b25f568de3c00000
https://www.brickfanatics.com/lego-seeking-afols-for-product-designer-role
https://www.newelementary.com/2020/06/lego-element-design-karsten-bunch-interview.html
https://www.brickfanatics.com/lego-designer-reveals-system-printed-stickers
https://www.lego.com/categories/adults-welcome/article/how-the-lego-collosseum-was-made
https://www.lego.com/en-us/legal/notices-and-policies/fair-play
https://en.wikipedia.org/wiki/Brickfilm
https://commons.wikimedia.org/wiki/Category:Lego_minifigures
https://download.osmand.net/wiki/Category:Lego_vehicles
https://library.ldraw.org/
https://library.ldraw.org/omr
https://www.ldraw.org/article/593.html
https://www.ldraw.org/legal-info.html
https://rebrickable.com/downloads/
https://rebrickable.com/api/v3/docs/
https://arxiv.org/html/2505.05469v3
https://github.com/AvaLovelace1/BrickGPT
https://huggingface.co/datasets/AvaLovelace/StableText2Brick
https://github.com/TobyLobster/ImportLDraw
https://pub.pg.edu.pl/en/publication/161724/photos-and-rendered-images-of-lego-bricks
https://arxiv.org/abs/2503.19990
https://arxiv.org/abs/2207.12572
