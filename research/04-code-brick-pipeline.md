# Code-driven brick pipeline (research, Oct 17, 2026)

Method and gaps: about 14 web searches (the shared search budget ran out, short of the 20 to 30 goal), then WebFetch was rate limited (HTTP 429). Versions and licenses were therefore checked by shallow git clones of the source repos, PyPI JSON, and local tests (bpy 5.2.2 on a 2 vCPU, 7 GB Linux box, plus a Jan 2023 LDraw mirror because ldraw.org downloads are blocked here). [U] = unverified or inferred.
Not read: BrickLink Studio EULA, Mecabricks terms, Legolization and Testuz papers, Blender 5.2 release notes, Bricker license text, LDraw complete.zip size. Sibling file 07-ip-and-brand-rules.md covers brand law; this file only flags license mechanics.

## 1. LDraw library, formats, license
- Current release: 2026-09 (161 new files, 121 new parts, 9 new primitives; ldraw.org homepage, no date shown). Library homepage states 17,164 unique shapes or patterned parts. Previous: 2026-02 (announced Mar 6, 2026; 187 new files, 107 parts, 4 primitives; zips briefly shipped as 0 KB, fixed Mar 7). Shadow-library README says "over 18000" official parts, a different count [U]. Naming is YYYY-NN. complete.zip size [U].
- License (two sources disagree, so honor both): CAreadme.txt (read in the Jan 2023 snapshot) says CC BY 2.0 ("CCAL 2.0"), header `0 !LICENSE Redistributable under CCAL version 2.0`. An LDraw admin (forum, release 2301 onward) says files are dual licensed CC BY 2.0 and CC BY 4.0, and the library will never be 4.0-only because some authors are unreachable. The legal page applies 4.0 only to website content.
- Attribution terms (paraphrased from CAreadme): credit the original author; for derivative works, crediting "The LDraw Parts Library" is accepted; tell recipients the license terms on any redistribution; commercial use and derivatives are allowed.
- Not derivative: rendered images (any 2D conversion), and model files that only reference official parts. Derivative: any alternative library converted from LDraw parts (so baked GLB/mesh part libraries need attribution plus license notice). Unofficial parts may be embedded in a model, but a model whose main purpose is bundling many of them counts as derivative.
- Ship a NOTICE file: "LDraw Parts Library" credit, CC BY version, link, and that LDraw is a trademark of the Jessiman estate. All-In-One-Installer is CC BY-ND 3.0 US, so never repackage it. Commercial renders: ldraw.org asks for credit and an optional donation, neither required.
- Formats (spec revision 1.0.2, 2012): `.ldr` (default model), `.mpd` (several files in one, `0 FILE name` blocks), `.dat` (parts). UTF-8, CRLF or LF. Line types: 0 comment/meta, 1 sub-file reference (color, position, 3x3 matrix, name), 2 line, 3 triangle, 4 quad, 5 optional edge line.
- Units: 1 LDU = 0.4 mm in tools (spec says about 1/64 in). Stud pitch 20 LDU (8 mm), brick height 24 LDU (9.6 mm), plate 8 LDU (3.2 mm), stud diameter 12 LDU, stud height 4 LDU (1.6 mm). Measured: part 3001 (2x4 brick) bbox = 80 x 28 x 40 LDU = 32 x 11.2 x 16 mm including stud.
- Coordinates: right-handed, -Y is up, so part origins sit at the stud plane and +Y points down. Blender is Z-up; importers handle it (ImportLDraw `realScale` 1.0 = real size). Rotation matrices should be 90 degree multiples for snapped builds [U].
- Colors: `LDConfig.ldr` lines `0 !COLOUR name CODE n VALUE #rgb EDGE #rgb` plus ALPHA, LUMINANCE, MATERIAL. Code 16 inherits the parent color, 24 is the edge color, direct colors use 0x2RRGGBB. The 2022-03 file has 204 colors (current count [U]). Studio keeps its own color table.
- Metadata in each part header: `!LDRAW_ORG`, `!CATEGORY`, `!KEYWORDS`, `!LICENSE`, `!HISTORY`, `BFC CERTIFY CCW`. Unofficial parts live in `ldraw/unofficial/` or ldrawunf.zip; per-file license varies [U].
- Primitives: `p/` standard, `p/8` low, `p/48` high resolution. Studs: plain `stud.dat`, logo variants stud-logo3/4/5 (bundled by ImportLDraw). Per 07, an LDraw forum member says the embossed logo is LEGO Group copyright [U]: default to plain studs. ImportLDraw defaults to plain, but ldr_tools_blender defaults to Logo4 (source: stud_type options Disabled, Normal, Logo4, HighContrast), so set Normal explicitly.
- Minifig parts (from ImportLDraw parents.txt): torso 973, head 3626 (printed faces 3626b + pXX), hips 3815, arms 3818/3819, hand 3820. Obsolete legs 3816/3817.

## 2. Blender importers and add-ons
- ImportLDraw (TobyLobster), v1.3.0 "Games" (source bl_info 1.3.0, Blender 4.0 minimum, last commit 2026-10-06). License: GPL-2.0-or-later (LICENSE v2, header says version 2 or later). README: tested to Blender 5.1.0; 1.3.0 fixed Blender 5 bugs (crease lines, transparent bricks, crash on a second import). 1.2.3 is the last release for Blender 2.81 to 3.6.
- Verified here: ImportLDraw 1.3.0 registers and imports headlessly under bpy 5.2.2 LTS (`import io_scene_importldraw; register(); bpy.ops.import_scene.importldraw(filepath=..., ldrawPath=...)`), 3 parts in 0.10 s, no errors. Reference driver: ldraw-mcp `blender_script.py` (`blender -b -P`).
- ImportLDraw options (from source): realScale, resPrims Low/Standard/High, smoothParts, look (photoreal or instructions), addGaps + gapWidthMM (default 0.1), curvedWalls, linkParts, positionOnGround, flatten, submodelCollections, animateSteps/framesPerStep, minifigHierarchy (default on), useUnofficialParts, useTextures (!TEXMAP prints), useLogoStuds (default off), instanceStuds, bevelEdges/bevelWidth/bakeBevels, addEnvironment, positionCamera. Also reads `.io`.
- ImportLDraw materials: Principled-based node groups for standard, transparent, fluorescent, rubber, chrome, metal, pearlescent, glitter, speckle, milky white, emission, fabrics. Same part+color shares one mesh by default; lsynth hoses and bands bundled.
- ldr_tools_blender (ScanMountGoat): MIT (repo LICENSE, SMG 2023). v0.5.2 (2026-09-29). Since 0.5.0 (2026-03-17) needs Blender 5.1+ and Python 3.13; 0.4.16 is the last for Blender 4.1+. Rust core (ldr_tools, PyO3 `ldr_tools_py`) is not on PyPI and has no prebuilt wheel: use the per-platform release zip or `cargo build --release`.
- ldr_tools_blender features: ldr/mpd and Studio `.io`, always instances by part+color, Geometry Nodes instance mode recommended above 10,000 parts, stud type and resolution settings cut vertices, gap option (since 0.3.0), procedural slope materials. Headless per DEVELOPMENT.md: `blender --background --python s.py` with `bpy.ops.import_scene.importldr(filepath=...)`. I did not run it; bpy-wheel use and Blender 5.2 are [U].
- ExportLdraw (cuddlyogre): GPL-2.0, last commit 2025-10-04, README names Blender 2.82/3.0 only, 5.x [U]. Only choice for Blender-to-LDraw export. LDR-Importer (le717) last updated 2018, dead. 3DToLD converts STL/OBJ/STEP to a `.dat` part (license not shown).
- Mecabricks Blender Lite: free, listed Blender 2.80 to 5.1, rev 3.1.13 (2024-09-04), "General license" on Superhive, imports Mecabricks models not LDraw. Terms unread [U]. Do not bundle.
- ldraw-mcp (musharna, MIT, v0.3.1 Sep 23, 2026): headless Cycles CPU renders, BOM, color lookup, part search. Docs say Blender 4.x, 5.x [U]. Good template for an agent render loop.

## 3. Builder apps, exports, headless
- BrickLink Studio 2.26.9_1 (winget, Sep 30, 2026), Windows and macOS, GUI only [U for any headless mode]. Part of the LEGO Group. Exports LDR/MPD, LXFML, POV-Ray, Collada .dae, CSV, BrickLink XML; imports .io, .ldr/.mpd, .lxf/.lxfml. Renderer Eyesight is a standalone 2018-era Cycles build fed a .dae (BrickNerd 2023), command-line launch possible on Windows only [U].
- Studio terms (IP help page): you generally own what you create and may show or sell designs, instructions and renders if no third-party IP is used; you may not redistribute or sell Studio or parts of it. So never ship Studio's part library or Eyesight; use LDraw. EULA unread [U].
- Mecabricks: browser workshop, free Blender add-on, paid server renders (a 2018 forum post said free renders cap at 1,000 visible parts, current rules [U]). Terms page failed to load [U]. Not a headless step.
- LeoCAD: GPL-2.0, source version 26.9.0, last commit 2026-10-17. CLI: `leocad -i out.png -w 1280 -h 720 -l <lib> --viewpoint home|--camera-angles lat lon --fov 30 -ss <stud style 0-7> model.ldr`; also `-obj -3ds -dae -csv -html`. Image export uses an offscreen GL context; still needs OpenGL (Mesa or xvfb) [U]. Best for fast clay-free previews.
- LDCad 1.7 beta 2b (2026-03-15), Windows and Linux, no CLI documented, license not stated (homepage: all rights reserved) [U]. Its snap data is the separate LDCadShadowLibrary (section 4).
- LPub3D: GPLv3, 2.4.9.86.4133 (SourceForge, 2025-03-19), Windows/Linux/macOS, listing mentions batch and headless modes and Docker [U, flags unread]. Makes step-by-step PDF/PNG instructions using POV-Ray or LDView. Use only if instruction pages are a product.
- Bricksmith: BSD-2, macOS only. LDPartEditor: MIT, Java (build needs JDK 25), part authoring. LDView (tcobbs, active Oct 2026): viewer, license [U].

## 4. Python and Rust libraries, connectivity, validity
- pyldraw3 1.7.0 (hbmartin, Aug 19, 2026; repo commit 2026-10-14): GPL-3.0-or-later, Python 3.12+, descended from GPL pyldraw. Verified: installs beside bpy 5.2.2 in one venv (numpy 2.5.4). `Parts.get("<lib>/parts.lst")` then `bounding_box("3001")` = 80x28x40 LDU, `stud_positions` = 8 studs, `connections("3001")` = 19 typed contacts (stud receptacles, tubes).
- pyldraw3 CLI: `ldraw validate [--strict]`, `inspect` (world bounds, contacts, gaps, JSON), `bom`, `render` (LDView or LeoCAD), `instructions`. Reads LDCad SNAP_CYL/CLP/FGR/GEN/INCL/CLEAR and Studio connectivity JSON. Ships a `lego-model-builder` agent skill. Needs a `parts.lst` at the library root (my hand-made list worked on the mirror).
- Dead or thin: pyldraw 0.8.2 (2019, GPL, Python below 3.7); ldraw 0.11 (2008, GPL); ldraw-py (michaelgale, MIT, last commit 2023-05-16, not under that name on PyPI [U]); weldr (Rust, MIT, 0.3.1, 2024); ldr_tools (Rust, repo MIT); ldraw.rs; node-ldraw.
- Connectivity data: LDCadShadowLibrary (Roland Melkert) is CC BY-SA 4.0 (ShareAlike), 3,350 part patch files + 87 primitive files, last commit 2026-09-10, `!LDCAD SNAP_*` lines in same-named .dat files. Share-alike makes bundling into a proprietary product risky; load it from a user-supplied path instead. Primitive inference (pyldraw3) needs no extra data.
- Stud-grid validity [U, my method]: snap positions to 20 LDU in X/Z and 8 LDU in Y, rotations to 90 degree multiples, then match stud centers to tube/receptacle centers within 0.5 LDU. Collision: AABB broad phase from pyldraw3 geometry, then mesh test (trimesh or FCL [U]) with a small interference tolerance for clutch.

## 5. Conversion and generation
- Bricker (Superhive, $65, GPL): v3.2.1 (2026-01-25), listed Blender 2.93 to 5.0, so 5.1/5.2 [U]; makes its own bricks in Blender (not LDraw parts), bevel, animation of sims, brick count and weight. Headless [U]. GPL, so run it but do not bundle.
- BrickGPT `mesh2brick` (MIT, in the BrickGPT repo): `uv run mesh2brick in.glb out.ldr`, outputs JSON/TXT/LDR; deps gurobipy, open3d, networkx. Uses the 8-brick set below. Older: brickify (STL to bricks), lsculpt, milsgen (heightmaps), Legofy (1x1 mosaics), openscad-ldraw.
- Papers found but unread [U]: Legolization (Luo et al., SIGGRAPH Asia 2015, doi 10.1145/2816795.2818091) and Testuz et al. 2013 (EPFL, constructable brick sculptures). Common recipe [U]: voxelize, tile with greedy or split-and-remerge bricks, penalize aligned seams, check connectivity, add stabilizers. BrickGPT's dataset used split-and-remerge with randomness.
- BrickGPT / LegoGPT (CMU, Pun et al., arXiv 2505.05469, ICCV 2025; README claims Best Paper): code, model and StableText2Brick data are MIT. Llama-3.2-1B-Instruct plus LoRA (gated base model, HF_TOKEN needed; Meta terms on the base apply [U]). 47k structures, 28k objects, 21 ShapeNet categories, 20x20x20 grid, 1-unit-tall bricks.
- BrickGPT brick set (brick_library.json): 1x1 (3005), 1x2 (3004), 1x4 (3010), 1x6 (3009), 1x8 (3008), 2x2 (3003), 2x4 (3001), 2x6 (2456). Last commit 2026-05-21; no 2026 model update seen. Depends on bpy (numpy<2) and gurobipy.
- LegoACE (VAST-AI, SIGGRAPH Asia 2025, MIT code and weights): 28 brick types, 20 rotation classes, text (CLIP) and 4-view image (DINOv2) conditioning, LDR out, no stability check, commit 2026-07-11, needs Blender 4.2+ and ImportLDraw for GLB.
- Others: Prompt-to-Parts (Noever, arXiv 2512.15743: LLM writes Python that emits LDraw, no physics, code release not stated); ObjectAssembler (HF Space, MIT, GPT + VQ-VAE + CLIP); Image2Lego (octree voxel autoencoder, code [U]); LDraw Nova (Anteloc, article dated Oct 3, 2026, license unnamed, speed claims unverified). No stability-aware 2026 successor found [U].

## 6. Buildability and stability
- BrickGPT definitions: valid = parses, brick in library, inside workspace, no collision; stable = every brick scores above 0 in a force and torque program (contact forces, friction limit, Newton's third law), solved with Gurobi in about 0.35 s for under 200 bricks. Rollback cuts the build back to the first unstable brick, then resumes.
- Reported numbers: LGM 25.2% stable, LLaMA-Mesh 50.8% stable, 5-shot LLaMA 1.2% stable; LegoGPT's own row was cut off in my fetch [U]. Authors also built outputs by hand and with a robot arm.
- Source check: the fallback "connectivity" test only links bricks in adjacent layers whose footprints overlap, then asks whether each reaches the ground layer (O(N^2) graph). It ignores stud/tube geometry, so it cannot validate arbitrary LDraw parts (slopes, Technic, offsets).
- Licensing trap: BrickGPT physics needs gurobipy. Pip wheels carry a size-limited trial license, academic use is free, production needs a paid license [U]. For a proprietary plugin, write our own equilibrium solver (linear/quadratic program with HiGHS or OSQP [U]) or ship connectivity-only.
- Recommended ladder [U, my design]: L0 parse; L1 all parts in library; L2 collision; L3 grid snap; L4 stud-contact graph (typed contacts from pyldraw3, not footprint overlap); L5 everything reaches a base plate; L6 static equilibrium with friction for display builds; L7 render-and-look check (ldraw-mcp style).
- Display-only scenes (renders, not instructions) may skip L6, but floating parts and interpenetration (L2, L5) should still fail CI; ldraw-mcp's README names crossed matrices, floating plates and sunken windows as typical export bugs.

## 7. Scale and performance (measured, bpy 5.2.2, Cycles CPU, 2 vCPU, 7 GB)
- Test brick: proxy with 264 verts, 150 faces (box + 8 studs), 320x180, 1 spp, no denoise, no bevel. Empty scene peak RSS 466 MB.
- Linked objects sharing one mesh: 10k = 942 MB; 100k = 5.2 GB peak, 6.5 s build, 7.3 s depsgraph, 10.4 s first frame. About 48 KB per object. Does not reach 1M on this box.
- Geometry Nodes Instance on Points (one source brick): 10k = 481 MB; 100k = 631 MB, 0.7 s frame; 1M = 2.2 GB peak, 8.3 s frame. About 1.7 KB per instance. Confirms the ldr_tools advice: use GN instances above 10k parts.
- EEVEE with no GPU in this container: EGL errors, 34 s for a 100-instance frame, a 100k run timed out at 500 s. Use Cycles CPU for CI and GPU Cycles for finals [U on cause].
- ImportLDraw vertices for one 2x4 brick (3001): Low/no logo/no bevel 248; Standard 472; High 1,368. Baked bevel: Low 1,728, Standard 2,008, High 5,592. Logo studs: Standard 5,040 (10.7x plain), with bevel 6,728; High+logo+bevel 10,312. Import time per brick type 0.06 to 0.4 s.
- Studs: ImportLDraw `instanceStuds` makes one Blender object per stud (its own warning: slow with many studs), so leave it off; ldr_tools resolves the stud primitive per part (stud type Disabled, Normal, Logo4, HighContrast) so studs stay inside the shared mesh; its defaults are gap on and scene_scale 0.01. A Geometry Nodes stud pass (one stud mesh instanced on points taken from pyldraw3 `stud_positions`) is a design option [U, untested].
- Blender 5.2 release page lists faster EEVEE in instance-heavy scenes and a Cycles texture cache; I could not test either (no GPU here) [U].
- Memory per unique part is small (thousands of verts); cost is per instance, so a scene of 1M bricks with 300 unique part+color pairs is feasible via GN [U beyond 1M]. Per-part texture/print images add memory.
- LOD plan [U]: hero shots High prims + bevel + (optional) logo-free studs; mid shots Standard + bevel; wide shots Low + no bevel; extreme wide merge by color. Pick LOD per shot in the shot spec, not per frame.
- Render time for real brick scenes: no benchmark found. Reuse 03-blender-3d-pipeline estimates (Cycles 5 to 30 s/frame at 1080p) as [U]; expect more for glossy ABS with transmission and bevels.

## 8. Rigging brick-built figures
- Minifigs are rigid parts. ImportLDraw `minifigHierarchy` parents objects using parents.txt (object parenting only, no armature, no limits; verified in source). parents.txt is GPL-2.0 data; re-derive pivots from part geometry via pyldraw3 to stay clear.
- Joint pivots (LDU, from parents.txt): torso origin; head attach (0, -24, 0); shoulders (+/-15.4, 8, 0); hips (0, 32, 0); leg attach (0, 12, 0) from hips; hand at arm 3818/3819 (+/-5, 19, -10) and arms 975/976 (+/-22.2, 21.0, -10.5). Multiply by 0.0004 for meters, then convert axes.
- Recommended rig [U, my design]: armature built by script, one bone per rigid part (head, torso, hips, 2 arms, 2 hands, 2 legs), parts parented to bones with `parent_type='BONE'`, Limit Rotation constraints (neck about 45 degrees, shoulders full swing, hips about 90 forward and less back, hands twist). IK optional. Squash is faked by scale on the pose bone; plastic does not bend.
- Hand and face swaps: swap the part (head 3626b + print variant, hand 3820 or accessories at the hand attach points) or swap a TEXMAP decal. Do mouth swaps on twos like the clay studio's replacement mouths (keeps the stop-motion read).
- Existing rigs: no verified rig for Mecabricks or community Blender minifigs this session [U]; Studio exports posable parts as MPD [U]. Treat any downloaded rig as unlicensed until read. Our own rig file is ours, so it can be MIT.
- Non-figure builds (creatures, vehicles): pose by transforming sub-assemblies (MPD submodels become collections via `submodelCollections`); use lsynth (bundled in ImportLDraw) for hoses and bands.

## 9. Reproducible headless pipeline
- Pin: `bpy==5.2.2` (Sep 15, 2026; GPL-3.0; Python exactly 3.13; wheels for Linux glibc 2.28+ x86-64, macOS 11+ arm64, Windows x64/arm64; about 880 MB installed). Blender 5.2 LTS shipped Jul 14, 2026, supported to Jul 2028. Same wheel as the clay studio, so one image can serve both.
- Pin the rest by hash: ImportLDraw git SHA (HEAD f05c840 is 1.3.0), pyldraw3 1.7.0 (coexists with bpy; verified), ldr_tools_blender 0.5.2 zip, LDraw release name (2026-09) with SHA-256 of complete.zip and LDConfig.ldr, LDCad shadow SHA, seed for Cycles and all sampling. BrickGPT's own pins (numpy<2, older bpy) conflict, so run it in a separate venv or container [U].
- What broke (verified in bpy 5.2.2): `modifier["Socket_1"] = x` now raises TypeError for Geometry Nodes inputs; use `md.properties.inputs.Socket_1.value = x` (identifier as attribute). Render engine list has only `BLENDER_EEVEE` (no `_NEXT`). `Scene.use_nodes` and `World.use_nodes` still work but warn (removal in 6.0). Compositor tree is `scene.compositing_node_group`, not `scene.node_tree`.
- What broke (release notes): 5.0 moved `bpy.props` data out of ID properties (so `scene["cycles"]` style access fails), made bundled modules private, and moved Python to 3.13; native modules such as ldr_tools_py must be rebuilt per Python. 5.2 also changed Compare and Random Value socket identifiers. Older Blender: ImportLDraw 1.3.0 is Blender 4.0+, ldr_tools 0.5.x is 5.1+.
- Install: `uv venv --python 3.13 && uv pip install bpy==5.2.2 pyldraw3==1.7.0`; ImportLDraw as a module on `sys.path` named `io_scene_importldraw`; library via `LDRAW_LIBRARY_PATH`; run `python -I build.py` (no Blender binary needed).
- CI tests: (1) `ldraw validate --strict` on every LDR; (2) import smoke test counting objects and vertices per part; (3) bbox of 3001 must equal 32 x 11.2 x 16 mm; (4) Cycles CPU 16 spp golden-frame diff with tolerance; (5) buildability ladder metrics on fixed models; (6) scan `!LICENSE` headers and fail on unknown ones; (7) nightly run on the next Blender point release.
- Sandbox note: ldraw.org and library.ldraw.org were blocked here, so production must fetch the library from an allowed mirror or vendor a pinned zip.

## 10. Recommended stack and license blockers
- Core: bpy 5.2.2 (Cycles) + ImportLDraw 1.3.0 for finals (richest materials, minifig parenting, bevels, gaps, TEXMAP prints) + ldr_tools_blender 0.5.2 for huge scenes (GN instancing). Authoring: Claude writes LDR via pyldraw3 as an external CLI. Preview: LeoCAD CLI. Agent loop: ldraw-mcp pattern (render, look, fix).
- Library: official LDraw release 2026-09 pinned, plain studs, with NOTICE. Generation: start with scripted designs and mesh2brick; treat BrickGPT/LegoACE as optional idea generators, never as the stability authority.
- Blocks bundling in an MIT or proprietary plugin: ImportLDraw (GPL-2.0+), ExportLdraw (GPL-2.0), pyldraw3 (GPL-3.0+), Bricker (GPL), LeoCAD (GPL-2.0), LPub3D (GPL-3.0), bpy itself (GPL-3.0; Blender says published scripts that use it must be GPL-compatible), LDCad shadow data (CC BY-SA 4.0), LDCad itself (no license found). Use them as separate processes or user-installed dependencies, never linked or copied in.
- Safe to bundle with notice: ldr_tools_blender and ldr_tools (MIT), BrickGPT code/data, LegoACE, ldraw-mcp, weldr, ldraw-py (MIT), Bricksmith (BSD-2), LDraw parts (CC BY, attribution). Watch Llama terms on BrickGPT weights and Gurobi.
- Note on Blender GPL: a pure bpy script is itself a GPL-bound work if distributed (per the Blender license page), so keep proprietary logic outside the bpy process boundary (CLI/JSON spec in, frames out) [U legal view, ask counsel].

## 11. Biggest open questions
- Does ldr_tools_blender 0.5.2 run under the pip bpy wheel and Blender 5.2 headless, and how fast is a 100k-part import? (needs the release zip or a Rust build)
- Which license governs the current library files for attribution text (2.0, 4.0, or both)? Read CAreadme in the live 2026-09 zip.
- Studio EULA and Mecabricks terms for commercial renders; any verified, licensed minifig rig.
- A stability solver without Gurobi that matches BrickGPT's labels; how well L4 contact graphs agree with it.
- Real render times and memory for 100k to 1M brick scenes with bevels and transmission on GPU.

## Sources
https://www.ldraw.org/ (2026-09 update, part count)
https://forums.ldraw.org/post-60087.html (2026-02 update)
https://forums.ldraw.org/post-47764.html (dual license thread)
https://www.ldraw.org/docs-main/licenses/legal-info.html
https://www.ldraw.org/article/218.html (file format 1.0.2)
https://github.com/TobyLobster/ImportLDraw (cloned, source read)
https://github.com/ScanMountGoat/ldr_tools_blender (cloned; CHANGELOG, DEVELOPMENT.md)
https://github.com/hbmartin/pyldraw3 and https://pypi.org/project/pyldraw3/
https://github.com/AvaLovelace1/BrickGPT, https://arxiv.org/abs/2505.05469, https://huggingface.co/cmu-gil/LegoGPT
https://github.com/VAST-AI-Research/LegoACE, https://arxiv.org/pdf/2512.15743v1
https://github.com/RolandMelkert/LDCadShadowLibrary, https://forums.ldraw.org/post-60305.html
https://github.com/musharna/ldraw-mcp, https://pypi.org/project/bpy/
https://www.blender.org/download/releases/5-2/, https://developer.blender.org/docs/release_notes/5.2/python_api/
https://studiohelp.bricklink.com/hc/en-us/articles/6502197862679-Exporting-to-other-formats
https://studiohelp.bricklink.com/hc/en-us/articles/6610276581015-IP-Guidance-Statement
https://bricknerd.com/home/hacking-studio-how-to-get-better-renders-like-blender-11-28-23
https://www.leocad.org/docs/cli.html, https://github.com/leozide/leocad, https://sourceforge.net/projects/lpub3d/
https://superhivemarket.com/products/bricker, https://www.mecabricks.com/en/shop/product/2
