# Look development: plastic, light, lens, color and post

Owners: the **Look Development Supervisor** owns materials, wear and the screen test; the **Director of Photography** owns lens and light; the **VFX Supervisor** owns post. Sources: research/05 (plastic look dev), research/01 sections 4 and 6. Every number marked [U] is a starting point to fit against Chris's brick photos (section 9), never a fact. No measured gloss, bevel radius or IOR for these bricks was found (research/05).

## 1. The look in one paragraph

Every frame should read as a macro photograph of real plastic bricks on a table: true-size studs and bevels catching soft light, hairline dark gaps between bricks, bricks a hair off square because hands built them, light glowing through thin white and yellow walls, transparent parts that pipe light to their edges, a few fingerprints and scratches only where hands would have been, depth of field that comes from a real lens at real distance, and colors that look like the bricks in a kid's bin, not like screen RGB.

## 2. Materials (Blender 5.x Principled BSDF; introspect socket names on the pinned build)

**Solid plastic v1** (research/05 section 1)
- Base Color: linear albedo fitted from photos (section 7). Cap white near 0.80 to 0.90 and saturated channels near 0.85 to 0.90; black is about 0.015 to 0.03, never 0.
- Metallic 0. IOR 1.53 [U]. Roughness 0.16 new with plus or minus 0.03 noise at 2 to 10 mm; 0.28 worn, scuff patches 0.40 to 0.55.
- Subsurface Random Walk: weight 0 for black and navy, 0.15 to 0.30 for mid colors, 0.30 to 0.50 for white, yellow, tan and light colors; scale 0.0005 to 0.001 m; radius tilted toward the base hue. Real walls are 1.6 mm, so backlit light parts glow at walls and studs.
- Optional coat for a factory-fresh wet look (set look only): weight 0.3 to 0.6, roughness 0.08 to 0.12.

**Transparent**: Transmission 1, roughness 0.03 to 0.06 new, 0.10 to 0.25 worn, IOR 1.52, pale base tint, plus Volume Absorption for depth (sigma = -ln(T)/t). Keep the hollow underside so light pipes to edges and studs. Cycles glass casts black shadows; use the shadow-ray trick for colored shadows [U]. Fluorescent transparent parts get a small masked emission (0.02 to 0.10) since Cycles has no fluorescence.

**Pearl and metallic-look**: dielectric base plus flake mask (0.1 to 0.2 mm cells, 30 to 60% coverage, flake metallic 0.6 to 0.9, tilt up to 25 degrees). **Chrome**: metallic 1, roughness 0.03 to 0.08, plating chips at edges; always give it something to reflect, or it renders black. **Glitter and speckle**: per the LDraw spec (FRACTION, SIZE in LDU, 1 LDU = 0.4 mm). **Rubber tires**: roughness 0.5 to 0.7, dark albedo, no subsurface, a tread seam. **Prints**: satin roughness 0.30 to 0.45, raised 10 to 20 microns, wear at high points. **Stickers**: about 0.1 mm thick with an edge that catches dust.

## 3. Geometry at true scale

- Work in true metres. Stud pitch 8 mm, brick 9.6 mm, plate 3.2 mm, stud about 4.85 mm wide and 1.6 to 1.7 mm tall, walls 1.6 mm (research/05 section 2).
- **Gaps are real geometry:** 0.15 mm plus or minus 0.05 between neighbors (each side relieved 0.1 mm). At 4K a 0.2 mm gap is about 4 px at 1:5 and 2 px at 1:10, so it can't be faked with a texture. Gaps read near-black, never pure black. Pixel filter 1.5 to 2.0 px.
- **Bevels** [U]: outer edges 0.12 to 0.20 mm, stud rim 0.15 to 0.25 mm, stud base fillet 0.10 mm, 2 segments, plus or minus 30% per part. True size at every shot scale; enlarged bevels look like marshmallows.
- **Molding**: slight draft (0.3 to 0.5 degrees) and low-frequency warp in the normals (0.2 to 0.5 degrees at 10 to 20 mm) so highlights wobble slightly. Gate and ejector marks as per-part attributes on hero parts [U].
- **Assembly jitter** (the features found perfectly snapped models looked wrong; research/01 section 4). Starting values [U]: offset plus or minus 0.03 to 0.05 mm, yaw plus or minus 0.15 degrees, tilt 0.1 to 0.3 degrees, 20% of bricks unseated 0.05 to 0.15 mm, at most 3% unseated 0.3 to 0.6 mm. Sub-assemblies move together. Tilt of a degree or more reads broken. Seeded from part ID plus position so a rebuilt model looks the same.
- **Stud mark**: never the toy maker's logo (FAN_RULES.md). Blank studs read wrong at 1:3 or closer, where a stud is 170 px or more. So studs carry **our own house glyph**, about 3.5 mm wide and raised 0.05 to 0.10 mm, designed with the house mark; blank is acceptable at 1:5 and wider.

## 4. Wear and imperfection

A stack of layers per brick, each seeded per brick so nothing tiles (research/05 section 3):
1. Batch color: hue plus or minus 0.7%, saturation 3%, value 2.5% [U]; era mixing on purpose in the film look.
2. Molding: warp, gate, ejector, parting line.
3. Edge wear: curvature mask raises roughness, slightly desaturates.
4. Scratches at three scales, along drag directions, on stud tops and edges.
5. Crevice grime from occlusion.
6. Oils: fingerprints and smudges change gloss, not shape (bump 1 to 3 microns at most). Ridges resolve only at 1:5 or closer; wider, show only gloss smears.
7. Dust, lint, at most one hair per hero frame; dust bunches in corners.
8. Model level: oxidation and yellowing on old whites and transparent clear.

| Budget | Set look | Film look |
|---|---|---|
| Fingerprints | 1 in 10 hero bricks, faint | 1 in 5 hero bricks, never on every stud |
| Scratch streaks | under 5% of hero surface | under 15% |
| Dust | crevices only, barely | crevices and dark colors |
| Unseated bricks | 10% slight, none large | as in section 3 |
| Yellowing, cracks | none | allowed, on purpose |

Any pattern repeating on three or more bricks is a tell. Each layer must be removable in an A/B without the frame turning CG-clean.

## 5. Camera and lens (DP)

- **Real lens math in Blender** (research/05 section 4): for a real focal length f, f-number N and magnification m, set lens = f(1+m), aperture f-stop = N(1+m), focus distance = f(1+m)/m. This reproduces real macro blur.
- **Perspective is a choice.** A figure-eye portrait needs the camera about 70 mm from the figure, which is a wide macro "probe" lens (about 24 mm). A 100 mm macro at 340 mm looks telephoto-flat. Lens kit: 24 mm probe (inside the world), 50 mm (the features' "tiny crew" look), 100 mm macro (product and box-art angles).
- **Depth of field honestly shallow, with the features' cheat:** real macro depth of field is millimeters (about 4 mm at f/8 framing a figure at 70% height), so relax it so backgrounds still read [U]: magnification 0.15 to 0.30, effective f-stop 14 to 22, subject sharp, background 20 mm or more behind visibly soft. 7 to 9 blade bokeh. Diffraction softening above effective f/16.
- **Never fake tilt-shift.** A screen-space blur gradient ignores depth and is the classic cheap-miniature tell. Depth of field always comes from the renderer.
- **Focus stacking** for set-look board stills only: 3 to 5 slices over the subject. Film shots never stack.
- **Toy-eye height:** figure eye line about 32 mm; hero camera 28 to 36 mm above the floor, pitch 0 to plus 5 degrees; play view 15 to 30 degrees down. Avoid steep top-down plus uniform blur.
- Camera rigs and shake: MOTION.md section 3.

## 6. Light (DP)

- Size sets the look: big sources hide molding detail, small ones reveal scratches and prints (research/05 section 5).
- Starter [U]: key area light 100 to 200 mm at 400 to 600 mm; small rim light 30 to 50 mm; fill from white bounce cards, not ambient; HDRI only at low strength for reflections.
- **Practicals are bricks:** light bricks as small emissive spheres (1.5 to 2.5 mm radius) inside transparent housings; gobos through grille parts.
- Bounces: total 12, diffuse 6 to 8, glossy 4 to 6, transmission 12 when transparent parts are present. Saturated plastic bleeds color hard at millimeter distances.
- Caustics through transparent studs are noisy: prefer large lights, Filter Glossy around 1.0, clamp indirect; use shadow caustics only on hero floor rings if they work on the pinned build [U].
- Exterior: sun is 0.53 degrees at any scale, so add big soft sky fill, key to fill about 3 or 4 to 1, blue shadows.

**Light presets by output**

| Preset | Used for | Setup |
|---|---|---|
| `studio_box` | Idea and Set boards (set look) | Soft key upper left, small rim, white cards, clean gradient backdrop, soft contact shadow |
| `instructions` | Instruction sheets | Flat even light, no shadows, outlines (section 8) |
| `practical` | Style frames and film look | Motivated sources, light bricks, haze cards, colored rim |
| `daylight` | Exterior scenes | Sun plus sky, bounce from the ground plate |

## 7. Color (DP and VFX Supervisor)

- Hex values disagree across sources (red: C91A09, B40000, DE000D) and LDraw white FFFFFF isn't physical. Treat table values as hue targets and **fit albedo from Chris's photographed color row** (GOLD_SET.md), scored by DeltaE2000.
- View transform bake-off on a color chart plus 12 photographed bricks: Standard, Khronos PBR Neutral, AgX (base and punchy). Expect PBR Neutral to win color fidelity and AgX to win highlight roll-off; AgX is known to skew saturated yellows (research/05 section 6). Record the winner here.
- Render in a wide working space; fluorescent transparent colors exceed sRGB, so gamut-map last.
- Keep the key below about 1.1 on yellow or the red channel clips.

## 8. Post and render tiers

**Post (VFX Supervisor):** per-pose sensor noise (same seed within a hold), diffraction softening, delivery downscale, 0.5 to 2% veiling glare from backlit transparent and emissive parts, mild vignette, sub-pixel chromatic aberration, glare only from true over-range sources. Skip halation unless a production asks for a film-stock look. Every effect should be visible only when toggled off.

**Never:** tilt-shift gradients, heavy bloom on white bricks, uniform grain, big chromatic aberration, lens-dirt overlays, sharpening halos, denoiser-smoothed prints, moire on stud grids (pixel filter 1.8 to 2.2), motion blur.

| Tier | Engine | Use |
|---|---|---|
| Thumbnail | Workbench or EEVEE | Read tests, layout, silhouettes |
| Instructions | EEVEE flat shading with line art outlines | Instruction sheets, parts callouts |
| Preview | Cycles, low samples, denoised | Pop-throughs, board drafts |
| Final | Cycles, full samples, albedo and normal fed to the denoiser | Board finals, film finals |

## 9. The screen test (Look Dev Supervisor)

A fixed scene that every material and wear change renders before it ships, and the place we match Chris's photos:
- The same builds Chris photographed (GOLD_SET.md), at 1:2, 1:5 and 1:10.
- A color row of every palette color, a transparent red 1x1 round backlit, a chrome part, a tire, a printed face, a 2x4 at 1:2 under a raking light, a gray ball and a chrome ball.
- Scored against the photos: DeltaE2000 on flat patches, highlight size and falloff, backlit glow against wall thickness, gap darkness. Validated on a held-out light angle.

## 10. Look regression

Every shader, wear, light or post change re-renders the screen test and three approved board stills; differences above threshold need the Look Dev Supervisor's sign-off.

## 11. CG tells checklist

From research/05 section 8 and research/01 section 8. The critic panel and Look Dev check every board and shot:
1. Perfect alignment, zero gaps.
2. Gaps vanish or bevels balloon on wide shots.
3. No wear, or wear that tiles.
4. Uniform gloss.
5. Mirror-sharp tiny reflections on studs.
6. Fake depth of field or tilt-shift blur.
7. Wrong scale cues (textures, grains, droplets at the wrong size; camera too far for the perspective).
8. Identical color on every brick of a color.
9. White bricks that look like chalk (no subsurface).
10. Transparent parts that look like tinted glass (no absorption, no edge piping, black shadows).
11. Chrome that renders black.
12. Clipped primaries.
13. Impossible builds or colors (the validator catches these first).
14. Blank studs in close-ups; no molding marks on hero parts.
15. Over-processed post; denoiser plastic.
16. Moire or shimmer on stud grids.
17. Smooth volumetric smoke, fire or water instead of bricks.
18. Motion blur.
