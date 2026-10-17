# Plastic brick look development (research, Oct 17, 2026)

~32 searches and ~60 fetches. The shared search budget ran out, and the fetch proxy rate-limited (HTTP 429) late, so Kasson, closeuphotography.com, savazzi macro-lenses, photomacrography.net, the Laowa probe page, Brothers Brick and the Blender 3.2 Cycles notes were never read. Blender 5.x manual bodies return navigation only, so Blender specifics come from release notes, the 3.2 manual and memory.
[U] = unverified or inferred. "Derived" = my arithmetic (4K = 3840 px, 36x24 mm sensor, thin lens). No measured gloss, BRDF or edge-radius data for LEGO ABS was found: every roughness, SSS and bevel value below is a look-dev starting point to calibrate against photos (procedure in 9).

## 1. Material physics and Principled BSDF starting values
- Resin: ABS since 1963, about 80% of parts. Moulded near 232 C, cooled about 15 s; machine tolerance down to 10 um, moulds up to 20 um; about 18 per million fail inspection (Wikipedia Lego, Bricknerd). The popular "0.002 mm" figure was not found in anything readable; use 10-20 um. Raw ABS is milky ivory, so every solid colour is pigment-loaded and translucent to some degree (Wikipedia ABS).
- IOR: polycarbonate 1.584-1.586 (Wikipedia); PMMA about 1.49 (refractiveindex.info fit); ABS has no primary figure found, typically 1.53-1.54 [U]; an Arnold practitioner used 1.55 for base and coat. F0 is 4.0% at 1.50 and 4.65% at 1.55 (derived), so anything in 1.50-1.58 is invisible next to roughness and normals. Use 1.53 solid, 1.52 trans, 1.585 for old PC parts.
- Transparent parts: PC historically (Makrolon); since about 2018 mostly MABS (clearer, brighter colours, glitter; early MABS looked milky), from one source only (Bricknerd) [U]. Cellulose acetate (1949-70) and SAN windscreens can be ignored.
- Gloss: ABS grades span 40-96% gloss (SpecialChem). No public LEGO polish spec; the forum guess of SPI A3 is speculation. Target look [U]: softbox reflections clear, rounded, slightly soft-edged, never mirror-sharp.
- SSS is real and was a headline item: Animal Logic rendered bricks with SSS (dipole in 2013, Jensen single scatter added 2014) and said real bricks show strong SSS in close-ups (fxguide). Animation Mentor says plastic scatters so ray-traced SSS is needed; an Arnold user ran full-weight random-walk SSS with a warm radius. Real walls are 1.6 mm (Zoe Blade), so backlit white and yellow bricks glow at walls and studs.
- Solid ABS starter (Principled, 5.x names; introspect the live build [U]): Base Color = linear albedo; Metallic 0; Roughness 0.16 new (noise +/-0.03 at 2-10 mm), 0.28 worn with scuff patches 0.40-0.55; IOR 1.53; IOR Level 0.5; Specular Tint white; Coat 0 (optional Weight 0.3-0.6, Roughness 0.08-0.12, IOR 1.5 for a factory-fresh wet look); Sheen 0; Subsurface Random Walk (not Skin), Weight 0.0 black and navy, 0.15-0.30 mid colours, 0.30-0.50 white, yellow, tan, light colours; Subsurface Scale 0.0005-0.001 m, Radius (1.0, 0.6, 0.4) tilted toward the base hue; Anisotropy 0-0.3. 5.0 random walk is multi-bounce with fewer dark artifacts (Cycles 5.0 notes); 4.0 made subsurface colour follow base colour and Scale multiply Radius.
- Albedo: white ABS reflectance 0.80-0.90 [U], never 1.0 (LDraw white FFFFFF is a display convention); black ABS about 0.015-0.03, not 0 [U]; cap saturated channels near 0.85-0.9.
- Trans (Clear, Red, Blue...): Transmission Weight 1, Roughness 0.03-0.06 new, 0.10-0.25 worn (scratch haze, milky edges), IOR 1.52, Base Color a pale tint (it multiplies at both surfaces). Add Volume Absorption on the Material Output for depth: sigma = -ln(T)/t. T=0.5 over 9.6 mm = 72 /m; T=0.1 over 9.6 mm = 240 /m; a 1.6 mm wall needs 6x more (derived). Keep the hollow underside so total internal reflection lights edges and studs (light piping).
- Cycles glass blocks shadow rays, so trans bricks cast black shadows. Fix: Light Path > Is Shadow Ray drives a mix with a tinted Transparent BSDF (coloured shadow, loses caustics) [U]. Trans-Neon (fluorescent) parts glow beyond their diffuse colour because the dye re-emits; Cycles has no fluorescence [U]. Fake: Emission 0.02-0.10 x tint masked to thick regions and edges, or a +0.3 EV saturated boost in comp. LDraw LUMINANCE appears only on glow and opal colours and most renderers ignore it.
- Pearl and metallic: LDraw PEARLESCENT = LEGO metallic-look colours, METAL = real metal, MATTE_METALLIC = brushed. Pearl is flake-loaded ABS [U]. Model a dielectric base plus a flake mask (Voronoi cells 0.1-0.2 mm, 30-60% coverage) with Metallic 0.6-0.9, flake normal tilt up to 25 deg, Roughness 0.2-0.3. 5.0 thin film only for rainbow looks.
- Chrome: plated or metallised coating on ABS [U]. Metallic 1, Roughness 0.03-0.08 new, 0.12-0.25 worn; silver F0 (0.80, 0.80, 0.82), gold F0 (1.0, 0.77, 0.34) via Specular Tint; plating chips at edges and stud tops (curvature mask). Chrome only shows its surroundings: without visible cards or HDRI it renders black (a tell).
- Glitter and speckle (LDraw spec): GLITTER has a colour VALUE, FRACTION (surface share, sample 0.17), VFRACTION (volume share, sample 0.2) and SIZE in LDU; SPECKLE sample FRACTION 0.4, MINSIZE 1, MAXSIZE 3 LDU. 1 LDU = 0.4 mm [U, from 20 LDU = 8 mm pitch], so grains are 0.4-1.2 mm. Build: object-space Voronoi at 0.4 mm, per-cell random normal tilt to 30 deg, flake Metallic 0.8, Roughness 0.1-0.2.
- Rubber tyres (SEBS, SBS, TPV family per Bricknerd): Roughness 0.5-0.7, albedo 0.015-0.03, no SSS, Sheen 0.1, tread mould seam 0.05-0.1 mm, grey haze when aged [U]. Cloth capes: Sheen Weight 0.5-1, Roughness 0.8+, weave pitch 0.2-0.4 mm [U] (see research/15).
- Print and stickers: pad printing lays one ink colour per pass with drying between (New Elementary) and the silicone pad conforms to curved or raised areas (Inkcups); ink thickness is unpublished, 10-30 um [U]. Print: satin Roughness 0.30-0.45 vs 0.16 plastic, 10-20 um raised bump, wear starts at high points and edges as 0.1-0.5 mm chips. Stickers: vinyl about 0.1 mm [U] with an edge that catches dust and a different specular. Blender artists also advise raised decals with a subtle bump.
- Tools: ABS Plastic Materials (Superhive, Blender 2.93-5.0) has Dielectric (value boost, colour randomness, two roughness values with a mix, speckle, fingerprints and dust, SSS), Transparent (absorption) and Bump (noise, waviness, procedural scratches) groups; needs UVs. ImportLDraw (TobyLobster) has PBR plastic, trans, rubber, chrome, metal, pearl, glow, glitter, speckle and fabrics; bevels baked as wide as will fit, optional gaps (size unstated), optional slightly concave walls, emissive light bricks. LDD, MLCad and LeoCAD exports are triangulated with no UVs, so use object-space or per-brick-offset coordinates.

## 2. Geometry at true scale
- Nominal: pitch 8 mm, brick 9.6 mm, plate 3.2 mm, stud 4.8 mm wide, stud 1.6-1.7 mm tall (Brick Owl 1.7; Zoe Blade and Brighton Toy Museum 1.6; varies by era), walls and ceiling 1.6 mm. Real studs mic at 4.87-4.90 mm (Brighton), so use about 4.85 for the look.
- Clearance: each side is relieved 0.1 mm, so width = 8n - 0.2 mm (Zoe Blade, Brick Owl, Brighton). Measured real 2-wide and 4-wide bricks are 15.9 and 31.9 mm (Brighton), and Bartneck measured 0.2 mm between side-by-side bricks. So the real neighbour gap is 0.1-0.2 mm total. Use 0.15 mm +/-0.05.
- Pixel footprint (derived, 4K, 36 mm sensor) at 1:1, 1:2, 1:3, 1:5, 1:10: 9.4, 18.8, 28, 47, 94 um/px. A 0.2 mm gap = 21, 11, 7, 4.3, 2.1 px. A 0.15 mm bevel = 16, 8, 5, 3, 1.6 px. A 70 um hair = 7.5, 3.7, 2.5, 1.5, 0.7 px. A 0.43 mm fingerprint ridge period = 46, 23, 15, 9, 4.6 px. A stud = 512, 256, 171, 102, 51 px. Gaps must be real geometry down to 1:10; ridges only resolve at 1:5 or closer.
- Gap look: a 0.15 mm slot 9.6 mm deep reads near-black except where light leaks through translucent walls. Keep it dark, not pure black, and use a 1.5-2.0 px pixel filter so sub-2 px gaps do not alias.
- Bevels: no measured radius found. Animal Logic's library was 2000+ subdivision-surface bricks, and artists say bevels are what catch light and sell plastic (Blender Artists). Start [U]: outer edges 0.12-0.20 mm, stud rim 0.15-0.25 mm, stud base fillet 0.10 mm, 2 segments, +/-30% per part. Keep true size at every shot scale; enlarged bevels read marshmallow-soft. Bake into the mesh for the silhouette.
- Draft and warp [U]: moulded walls carry 0.3-0.5 deg draft (ImportLDraw's concave-walls option suggests others do this). Real moulding warp shows in highlight distortion (Arnold thread): low-frequency normal noise at 10-20 mm scale, 0.2-0.5 deg amplitude on the coat or base normal.
- Assembly jitter: Animal Logic found perfectly snapped models looked wrong and built a custom jitter for angle variation (3D World); their library notes a small procedural transform jitter. Starter [U]: X/Y offset +/-0.03-0.05 mm (inside the clearance); yaw +/-0.15 deg; tilt +/-0.1-0.3 deg (0.3 deg over a 32 mm brick lifts one end 0.17 mm, derived); 20% of bricks unseated 0.05-0.15 mm; at most 3% unseated 0.3-0.6 mm; move sub-assemblies coherently. Tilt of 1 deg or more reads broken because clutch holds.
- Manufacturing marks (sizes not found) [U]: gate marks are small round nubs or dimples near 0.5-1 mm, and LEGO moves them between mould versions (plates from stud to side, tiles from edge to underside; Eurobricks), so make it a per-part-ID attribute. Ejector pin rings 2-4 mm and about 0.05 mm deep on undersides. Parting line a 5-20 um ridge, visible only in raking macro light. No sprues survive on finished parts.
- Stud logo is a trademark, so do not copy it. Generic options: (a) blank studs (reads wrong at 1:3 or closer, where a stud is 170 px); (b) an invented studio glyph or ring-and-dot mark, about 3.5 mm wide and raised 0.05-0.10 mm [U], same orientation across one part; (c) a shallow recessed ring. ImportLDraw can add the real logo; we should not.

## 3. Imperfections and wear
- What the films did: Animal Logic used one LEGO shader with texture variants assigned randomly or by hand per channel (plastic warp, roughness, decals), procedural edge wear and grunge, semi-procedural scratches tied to stud detail, PTEX-baked occlusion for wear, and a model-level pass for grunge and oxidation. Macro photos of real bricks under varied light drove the shaders. Reference rules: exterior bricks oxidised like sandbox toys, interiors like a basement after about 10 years, plus fingerprints, thumbprints, dust and hair (Animation Mentor). Smudges landed mostly on Emmet's shiny plastic (LEGO.com). Batman moved to per-face textures when ptex hit its limits (SIGGRAPH 2017).
- Layer stack per brick [U]: (1) batch colour jitter; (2) mould: warp normal, gate, ejector, parting line; (3) edge wear: curvature mask raises roughness and slightly desaturates (Arnold thread); (4) scratches at three scales; (5) crevice grime from AO; (6) oils: fingerprints and smudges; (7) dust, lint, hair; (8) assembly-level oxidation and yellowing.
- Fingerprints: skin oil changes the reflection, not the shape. Drive Roughness (+0.1 to +0.3), Coat and slight darkening; bump 1-3 um at most. Ridge period 0.41-0.46 mm (NCJRS, via research/10); needs 4+ px per period, so 1:5 or closer; at wider scales show only patchy gloss smears. A touch is 10-16 x 14-20 mm (about 2x2 studs); drags run 5-30 mm. Most visible on black, dark blue, trans-clear and tiles; faint on white. Never tile one map across bricks: unique seed per brick.
- Scratches [U]: 5-30 um wide, 0.5-5 mm long, mostly sub-pixel, so they read as specular streaks only when light rakes. Cluster along drag directions on stud tops and edges; about 5-20 per cm2 for light play, 50+ for beaten bricks.
- Dust, lint, hair [U]: dust 10-50 um on up-facing surfaces and in crevices; lint 20-50 um x 1-3 mm; hair 50-100 um x 5-30 mm (3.7 px wide at 1:2). At most one hair or lint per hero frame. Animal Logic's Spawn tool bunched dirt in corners and thinned it in open areas (VFX Voice).
- Batch colour: pigment amounts reportedly vary between batches and within a run (Eurobricks, unverified); LEGO inspectors check colour and thickness variation (Wikipedia). Starter [U]: per-brick hue +/-0.7%, saturation +/-3%, value +/-2.5%; between eras the same-named colour can drift DeltaE 3-6 (old vs new Light Bluish Gray), so model era mixing on purpose.
- Yellowing: hits white, trans-clear, old gray and blue. Cause disputed (UV breaking brominated flame retardant vs ABS double-bond degradation). Takes months to years, is patchy, can be one-sided where sun hit, and leaves yellowed and white bricks side by side in one model (Eurobricks). Render: lerp white albedo (0.85, 0.85, 0.85) toward (0.80, 0.72, 0.50) by 0-40% using 10-40 mm noise plus a per-brick bias; drop gloss slightly; give trans-clear a pale amber absorption.
- How much is too much: Animal Logic's brief was light wear, as if children had played with them; wear was uneven, with smudges concentrated on a few shiny hero parts. Budget [U]: a visible fingerprint on at most 1 in 5 hero bricks and never on every stud; scratch streaks on under 15% of hero surface; dust mainly in crevices and on dark colours; any pattern repeating on 3+ bricks is a tell; each layer should be removable in an A/B without the frame turning CG-clean. At 1:10 keep only roughness variation, batch colour shifts and crevice dust.

## 4. Camera and lens at toy scale
- Framing (derived): a 40 mm figure fills 70% of frame height at m = 0.42 (1:2.4) and 40% at m = 0.24; a 200 mm diorama fills the width at m = 0.18 (1:5.6).
- DOF: total DOF = 2 N c (1+m) / m^2 (thin lens, pupil ratio 1). Effective aperture is N(1+m) (Savazzi), and at equal magnification the same f-number gives the same DOF on any focal length (Wikipedia). Derived, c = 0.030 mm: at f/8, 1:1 0.96 mm, 1:2 2.9, 1:3 5.8, 1:5 14.4, 1:10 53; at f/16, 1.9, 5.8, 11.5, 28.8, 106. Halve for c = 0.015 (4K critical). At m = 0.42 and f/8: 3.9 mm (1.9 at c = 0.015), so face sharp and shoulders soft.
- Diffraction (derived): Airy diameter about 2.44 x 0.55 um x N(1+m): f/11 at m = 0.5 is 22 um, about 2.4 delivery pixels; f/16 at 1:1 is 43 um. Add Gaussian blur sigma 0.4-0.6 px when N(1+m) is 16-24. Real macro shooters stay near f/8-f/11 nominal and stack for more.
- Blender mapping (derived; reproduces real blur): lens = f(1+m), aperture_fstop = N(1+m), focus_distance = f(1+m)/m to the entrance pupil. Example f 100, N 8, m 0.42: lens 142 mm, f/11.4, focus 338 mm, horizontal FOV 14.4 deg. Same recipe as research/11. Add breathing, vignette, CA in comp.
- Perspective trap (derived): a human portrait at 3 m on 35 mm, scaled 1:44, puts the camera 68 mm from a figure, which needs about a 24 mm macro probe lens (Laowa 24 mm f/14 probe class; specs unverified). A 100 mm macro for similar framing sits at 340 mm and compresses depth. f 60 at m 0.5: 180 mm, 22.6 deg; f 24 at m 0.5: 72 mm, 53 deg. Choose the virtual lens on purpose; 60-105 mm macros look telephoto-flat.
- Fake tilt-shift reads cheap: a screen-space gradient ignores depth (towers cross it), edges halo, and display-referred blur turns bright highlights into dim blobs instead of bright discs. Animal Logic did lens blur in comp on the first film and cleaned deep-data artifacts, then moved DOF into the renderer and redistributed energy so out-of-focus highlights stay bright (fxguide, Batman paper). A perception study (36 viewers) found blur only convinces when it fits the subject. Real miniature crews shot T11-T16 to look big (Pro Video Coalition), so shallow DOF is the small-scale cue; make it depth-true with 7-9 blade bokeh.
- Honest-shallow with a cheat: Animal Logic noted a real macro camera would be very shallow and relaxed it so detailed backgrounds read (Animation Mentor). Starter [U]: m 0.15-0.30, N(1+m) 14-22, subject plane sharp, background 20+ mm behind visibly soft.
- Focus stacking: stills only (photographers stack when needed, Four Bricks Tall). CG: 3-5 slices over the subject merged over an unstacked background [U]. Fully sharp front to back reads as product CG. Stack halos and ghosting do not occur in CG, so add none.
- Lens character [U]: macro glass is flat-field with CA under 1 px, vignette gone by f/8 at high magnification, distortion under 1%.
- Toy-eye height (derived): eye line about 32 mm (80% of 40 mm). Hero camera 28-36 mm above floor, pitch 0 to +5 deg; play view 15-30 deg down. Avoid 60 deg top-down plus uniform blur (the fake-miniature cliche). Keep floor texture, horizon and 8 mm stud pitch in frame as scale cues.

## 5. Light at toy scale
- Angular size (derived): 300 mm softbox at 400 mm = 41 deg; 100 mm panel at 500 mm = 11 deg; 30 mm bulb at 500 mm = 3.4 deg; 5 mm LED at 300 mm = 1 deg; sun = 0.53 deg. Penumbra for a shadow thrown 20 mm: 15, 4, 1.2, 0.3 mm. Big sources hide moulding detail; small ones reveal scratches, gate marks and prints.
- Toy photographers (Brickset, BrickNerd, toyphotographers.com, Four Bricks Tall): diffuse everything (tissue, crepe or baking paper, mini softboxes); avoid frontal light on minifigs; backlight for separation plus a white card on the far side; build the light one source at a time; matte neutral or black velvet backdrops; daylight bulbs above 4500 K; bare lights glare off domed heads.
- CG starter [U]: key area light 100-200 mm at 400-600 mm (11-25 deg); small rim light 30-50 mm; fill from white bounce cards rather than ambient; HDRI only low-strength for reflections; Light Tree on.
- Light bricks and practicals: real light bricks are 3-5 mm LEDs in a translucent housing. Model as emissive spheres r 1.5-2.5 mm (strength by calibration) or spot plus gobo. The Lego Movie team photographed an LED behind a grating brick and made it a gobo (Animation Mentor). Batman had 20,000+ lights, and small lights made bright bokeh that drove in-render DOF.
- Bounce: at mm distances saturated plastic bleeds hard. Bounces [U]: total 12, diffuse 6-8, glossy 4-6, transmission 12 with trans parts.
- Caustics: path tracing can make them but it is inefficient and noisy (3.2 manual). Reflective and Refractive Caustics toggle separately; Filter Glossy start 1.0 and clamp indirect separately (manual); roughening helps. Small hard lights through trans-clear studs give fireflies. Options [U]: large area lights; Cycles shadow caustics (manifold NEE, since 3.2; flags on light, caster, receiver; smooth near-zero-roughness glass only) for hero floor rings; else light-linked spot plus gobo, or a comp pass; CPU path guiding.
- Glow: SSS in white and yellow walls and light piping in trans parts. Prefer Volume Absorption + SSS over emission, and avoid clipping.
- Sun and sky: 5.0 Sky Texture uses multiple scattering. The sun is 0.53 deg at any scale, so exterior toy shots look crisp: add big soft sky fill, key:fill 3-4:1 [U], blue shadows. Do not mix infinite HDRI with small fixtures without flags.

## 6. Colour
- Sources disagree: Red LDraw C91A09, LDD B40000, Peeron C4281B, LCP DE000D; Yellow F2CD37, FAC80A, F5CD2F, FEC400; Black 05131D, 1B2A34, 010101; White FFFFFF vs F4F4F4. The LDraw table dates from the 2013 LDConfig and is flagged as needing an update. No measured spectral data found. Treat as hue targets and fit albedo from photos (research/11 procedure).
- Linear values (derived): LDraw Red (0.584, 0.010, 0.003); Blue (0, 0.091, 0.521); Yellow (0.888, 0.611, 0.038); Green (0.019, 0.195, 0.048); Light Bluish Gray (0.352, 0.376, 0.397); Dark Bluish Gray (0.150, 0.156, 0.138); Black (0.0015, 0.0065, 0.0123) vs LDD Black (0.011, 0.023, 0.034). Yellow's top channel leaves 0.17 stop before 1.0, so a key much above 1.1 clips red in Standard.
- Blender view transforms: AgX is default since 4.0 and skews or desaturates saturated materials (issue 114988: yellow metal tinted red; maintainers call it a view-transform trade-off; Standard and Filmic show it less). Khronos PBR Neutral exists since 4.2: reproduces base colours 1:1 under grey light up to a threshold then compresses highlights, sRGB output only, built for well-exposed product scenes. ACES washes bright yellows, greens and cyans on sRGB (Khronos). 5.0 (Nov 2025) adds ACES 2.0, AgX HDR, Linear Rec.2020 working space and a Convert Colorspace node. Filmic status in 5.x [U].
- Recommendation [U]: A/B Standard, PBR Neutral, AgX (None and Punchy) on a ColorChecker plus 12 photographed bricks; score DeltaE2000 on flat patches and judge highlight roll-off. Expect PBR Neutral to win fidelity and AgX roll-off; apply the show LUT after. Animal Logic worked in linear P3-D60 with ACES display transforms and moved to ACEScg for out-of-gamut notes (fxguide). Neon trans colours exceed sRGB: render wide, gamut-map last.

## 7. Post
- Helps: per-pose sensor noise (research/11), diffraction softening, delivery downscale, veiling glare 0.5-2% from backlit trans and emissive parts, mild vignette, sub-pixel CA, low-strength Glare only from true over-1.0 sources (LED bricks, speculars).
- Halation is a film-stock effect: skip unless the show look is film. Animal Logic styled cinematography on 1970s-80s anamorphic lenses (3D World), a deliberate cheat, and ran character animation without motion blur for a stop-motion feel (VFX Voice).
- Screams CG: tilt-shift gradient; heavy bloom on white bricks; uniform grain; big CA; lens-dirt overlays; sharpening halos; denoiser smoothing of prints and speckle (feed albedo and normal, check detail, research/10 s8); moire on regular stud grids in wide shots (filter width 1.8-2.2); motion blur on held poses.

## 8. CG tells checklist (tell, then fix)
1. Perfect alignment, zero gaps: 0.15 mm gaps, jitter 0.03-0.05 mm and 0.1-0.3 deg, a few unseated bricks.
2. Gaps vanish or bevels balloon at wide shots: keep true size, filter 1.5-2 px.
3. No wear: the 8-layer stack, light play, uneven distribution.
4. Uniform gloss: roughness noise, scuff patches, per-brick variation.
5. Mirror-sharp tiny reflections (roughness under 0.08 on a 4.8 mm stud): roughness 0.12+, warp normals, key at 10 deg or more.
6. Fake DOF: render-time DOF, depth-true bokeh.
7. Wrong scale cues: true-size textures, weave and grains; match camera distance to perspective; droplets no smaller than a few mm [U].
8. One fingerprint or scratch tile on every brick: unique seeds, no tiling.
9. Identical colour on all bricks: batch jitter, era mixing.
10. White bricks look like chalk: SSS 0.3-0.5 with 0.5-1 mm scale.
11. Trans bricks look like tinted glass: volume absorption, hollow underside, edge piping, coloured shadow trick.
12. Chrome renders black: visible cards or HDRI.
13. Clipped primaries: albedo caps, PBR Neutral or AgX, key below about 1.1.
14. Impossible builds or illegal colour-part combos: legal parts and connections only (Animal Logic: every frame buildable).
15. Blank studs, no gate or ejector marks: generic stud mark, per-ID marks on hero parts.
16. Over-processed post: every effect only visible when toggled off.
17. Denoiser plastic: preserve micro-detail, re-add sensor noise per pose.
18. Moire and shimmer on stud grids: pixel filter and AA samples.

## 9. Look-dev procedure and open questions
1. Photograph a real set: red, yellow, white, black, Light Bluish Gray, a trans-red 1x1 round, 2x4 brick, 1x1 plate, tile, a pearl part, a chrome part, a tyre, with grey ball, chrome ball, ColorChecker, mm scale and a backlit 1.6 mm wall; locked light and camera, raw.
2. Recreate at true scale; fit albedo (DeltaE2000), roughness (highlight size and falloff), SSS (backlit glow vs wall thickness); validate on a held-out light angle (method as research/10 c).
3. Open: ABS IOR and real roughness; true edge radius; gate and ejector sizes; whether shadow caustics (MNEE) work in 5.x and their property names; exact 5.x Principled socket names; whether current LEGO trans is MABS; measured focus breathing of 100 mm macros; PBR Neutral vs AgX bake-off result.

## Sources
https://en.wikipedia.org/wiki/Lego
https://en.wikipedia.org/wiki/Acrylonitrile_butadiene_styrene
https://en.wikipedia.org/wiki/Polycarbonate
https://en.wikipedia.org/wiki/Depth_of_field
https://bricknerd.com/home/every-type-of-plastic-used-by-lego-2026-edition-3-12-26
https://www.specialchem.com/plastics/guide/gloss
https://refractiveindex.info/?shelf=3d&book=plastics&page=ABS
https://notebook.zoeblade.com/Lego_brick_dimensions.html
https://www.brickowl.com/help/stud-dimensions
https://www.brightontoymuseum.co.uk/index/Lego_dimensions
https://www.bartneck.de/2019/04/21/lego-brick-dimensions-and-measurements/
https://www.eurobricks.com/forum/forums/topic/190657-injection-marks-on-weird-places
https://www.eurobricks.com/forum/forums/topic/203287-discoloration-due-to-sun-exposure/
https://animallogic.com/portfolio/projects/tlm/
https://animallogic.com/wp-content/uploads/2023/06/The-LEGO-Movie-Bricks-Bricks-and-More-Bricks.pdf
https://animallogic.com/wp-content/uploads/2023/07/The-LEGO-Movie-Construction-Animation-And-Demolition.pdf
https://animallogic.com/wp-content/uploads/2023/06/Rendering-the-Darkness-Glimpse-on-the-Lego-Batman.pdf
https://history.siggraph.org/wp-content/uploads/2022/09/2017-Talks-Jebens_Evolving-Complexity-Management-on-The-LEGO-Batman-Movie.pdf
https://www.fxguide.com/?p=184504
https://www.animationmentor.com/?p=3062
https://www.pressreader.com/australia/3d-world/20140520/281724087577080
https://vfxvoice.com/the-building-blocks-behind-the-lego-movie-2/
https://www.lego.com/categories/adults-welcome/article/was-the-lego-movie-stop-motion
https://blenderartists.org/t/photo-real-lego-scenes/623669
https://answers.arnoldrenderer.com/questions/8973/lego-plastic-shader.html
https://superhivemarket.com/products/abs-plastic-materials/docs
https://github.com/TobyLobster/ImportLDraw
https://www.ldraw.org/article/299.html
https://www.ldraw.org/article/547.html
https://wiki.ldraw.org/wiki/User:Owen_Burgoyne/LDraw_Colour_Comparison_Chart
https://www.newelementary.com/2018/08/the-lego-minifigure-at-40-inside-factory.html
https://www.inkcups.com/blog/how-lego-bricks-are-printed-choosing-the-right-pad-printer/
https://savazzi.net/photography/macrodof.htm
https://www.provideocoalition.com/fake_tilt_shift_cinematography/
https://journals.sagepub.com/doi/abs/10.1068/id223
https://brickset.com/article/20303/how-to-make-a-cheap-but-effective-light-box-to-help-improve-your-lego-photography
https://fourbrickstall.substack.com/p/behind-the-photo-bard-y-time-excellent
https://toyphotographers.com/?p=26993
https://bricknerd.com/home/diy-guide-for-lego-photography-7-24-21
https://docs.blender.org/manual/pt/3.2/render/cycles/render_settings/light_paths.html
https://docs.blender.org/manual/pt/3.2/render/shader_nodes/shader/principled.html
https://developer.blender.org/docs/release_notes/4.0/shading/
https://developer.blender.org/docs/release_notes/5.0/cycles/
https://wiki.blender.org/release_notes/4.2/rendering/
https://itsfoss.com/news/blender-5-0-release/
https://projects.blender.org/blender/blender/issues/114988
https://khronos.org/news/press/khronos-pbr-neutral-tone-mapper-released-for-true-to-life-color-rendering-of-3d-products
