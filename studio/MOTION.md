# Motion: how bricks and figures move

Owners: the **Supervising Animator** owns the style guide; the **Animation TD** owns the engine; the **Brick FX Artist** owns effects motion. Sources: research/02 (brickfilm motion), research/01 sections 3 and 5. Numbers marked [U] are starting points to calibrate on Chris's gold clips, not facts.

Brick Studio reuses the clay studio's animation engine design (`rader-ai/clay-motion` `studio/MOTION.md` section 4): the beat sheet (`shot.yaml`), the timing compiler that bakes stepped keys into `exposures.json`, the contact solver, seeded secondary layers, the lip-sync mapper, the pop-through and the filmstrip review. Whichever studio builds the engine first builds it as shared code (ENGINEERING.md section 2). This file lists what changes for bricks.

## 1. The core idea

A toy can only move the way a hand could move it. Parts never bend, stretch or squash. Figures turn at their joints and nowhere else. Every pose is a pose you could set by hand, and the film steps from pose to pose the way stop motion does. The best brick features held this line on purpose and built their acting inside it; the one that loosened it (bent limbs, simulated water) is the one fans and critics marked down (research/01 sections 3 and 8).

## 2. Style guide v1 (24 fps)

**Cadence**
1. Characters on twos by default (12 poses a second). Never let the computer in-between; every exposure is a held pose (research/02 section 2).
2. Dialogue: bodies on fours or sixes, mouths on twos (research/01 section 3).
3. Fast action: sources disagree (threes and fours for roughness vs ones for clarity, research/01 section 3). Default twos; the Supervising Animator may tag a segment ones or threes, and motion QC treats an untagged change as a fail.
4. Camera moves on ones (as in clay).
5. Holds in multiples of two; 12 or more frames for an expression to register. Avoid 3 to 5 frame holds, which read as a mistake [U].
6. **No motion blur, ever.** Fast moves get brick blur: a short strip or cluster of bricks in the character's colors trailing the move for one or two exposures (research/01 section 3).

**The rigid figure** (research/02 section 3)
7. Joints: head turns, arms swing at the shoulder, hands twist at the wrist, legs swing at the hip. No elbows, no knees, no bending torso. Exact limits come from Chris's figures (GOLD_SET.md), stored in `rig/joint_limits.json`.
8. Anticipation without squash: move a little the opposite way first (lean back before reaching down, arm back before a throw), 2 to 4 frames.
9. Ease by step size: small step, grow to the middle, small last step. The clay starting curve (about 1 : 3 : 4 : 3 : 1) is a first guess [U].
10. Feeling lives in head tilt, lean, arm pose and the face print. Small head moves for shock; big moves for anger or joy.
11. A shrug is an arm move plus a torso shift plus a head tilt. Clapping or crossing arms gets turned away from camera so nothing passes through anything.
12. Allowed cheats, the way brick animators do it: swap in a replacement part mid-shot (a bent arm, a different torso, a head with a different face), hide a seam off camera, add a part that wasn't there (a spark, a puff). Swaps are logged in `shot.yaml` and reviewed like any pose.

**Walks, runs, jumps** (research/02 sections 2, 3, 9)
13. Feet step stud to stud and lock to the stud while planted. Step length is a whole number of studs. Sliding feet and "pigeon bob" (forward travel on only some frames) are the top amateur tells.
14. Walk step 8 to 12 frames on twos (4 to 6 poses); add body bounce, a hip twist and torso tilt. Small figures cover ground fast; don't slow them down to look cute.
15. Run: airborne frames with both feet off the ground, body tilted forward, body rises (not drops) when legs spread.
16. Jump: anticipation 4, rise 6 to 8 in plate-height steps eased 2, 2, 1, 1, apex hold 2, fall 6 eased 1, 1, 2, 2, landing settle 4 [U].
17. Sit: legs to 90 degrees at the hips, body drops onto studs, small head nod to settle.
18. Avoid walks where the story allows, as in clay: crop, cut away, or exit frame.

**Faces** (research/02 section 4)
19. Faces are prints. Each figure gets a face set: 6 to 8 mouths (closed, small open, wide open, O, wide smile, teeth on lip for F and V), 2 to 3 blinks, plus emotion variants of brows and eyes [U]. Built by the Print and Decal Artist as layered decals, so mouth, eyes and brows swap separately.
20. Mouth changes at most every 2 frames; closed shapes (B, M, P) land on the sound frame or one before; head or gesture starts 4 to 6 frames before a line.
21. A blink is a 2-frame closed print, 2 to 4 frames total. Printed faces have no half-lid.
22. Keep the little bumps and scratches real prints have; flat perfect faces read as CG.

**The builder's hand** (research/02 section 3, research/01 section 4)
23. Small step irregularity only on channels a beat touched (as in clay). Never on contacts, eyelines or holds.
24. Assembly jitter (tiny angle and gap errors between bricks) is a look setting, fixed per model, never animated (LOOK_DEV.md).

## 3. Engine changes from clay

| Clay engine part | Brick version |
|---|---|
| Puppet rig, about 25 bones, IK legs, stretch property | **Figure rig from parts.** Bones only at real pivots, read from part metadata by the Brick Librarian's catalog: head yaw, shoulder pitch per arm, wrist roll per hand, hip pitch per leg, plus root. FK only. No stretch property. |
| Replacement slots: mouth, brows, lids, eyes | **Slots:** `mouth`, `eyes`, `brows` (decal layers), plus `arm_l`, `arm_r`, `torso`, `head`, `hand_item` for part swaps |
| Mouth set from a parametric lip model | Printed mouth set from the Print and Decal Artist, mapped from Rhubarb's A to H plus X to the 6 to 8 printed shapes |
| Contact solver: feet raycast to floor | **Stud solver:** planted feet snap to the nearest stud position on the surface below and lock; steps land on whole studs; hands snap to bars and clips when gripping |
| Prop handoff with Child Of | Same, plus **attach and detach** of parts on studs (a figure picks up a brick, a part snaps onto a model), with a click event written for sound |
| Simulated props baked on twos | **Brick FX** (section 4) |
| Camera rig, clean moco | **Lens kit and camera rigs** with fixed focal lengths and per-rig shake: locked, dolly, crane, steady rig, handheld, vehicle mount. Pivots and mounts modeled physically, never a clean single-axis turn (research/01 section 6). On ones. |
| No build animation | **Builds itself** (section 5) |

## 4. Brick FX

Effects are parts. Water, fire, smoke, dust, clouds, sparks, light beams and explosions are made of real parts, swapped per exposure (research/01 section 5, research/02 section 6).

1. **Shape.** Either hand-authored key shapes (the Brick FX Artist builds the explosion at 3 to 6 moments) or a Blender simulation (smoke, liquid, rigid body) used only as a guide.
2. **Brickify each exposure.** Voxelize the shape at stud resolution; place real parts (1x1 rounds, 1x1 and 2x2 plates, 1x1 cones, transparent studs) at filled cells; map density or temperature to colors from a fixed FX palette:
   - fire: transparent yellow, transparent orange, transparent red, with an orange core
   - smoke: white, light bluish gray, dark bluish gray, black for heavy smoke
   - water: transparent light blue, clear, medium azure, white for foam
   - dust and debris: tan, dark tan, the colors of whatever broke
3. **Hold** each arrangement for 2 exposures by default (ones for fast debris if tagged). Never interpolate between arrangements.
4. **Jitter** each FX part's rotation slightly, seeded, so it reads as hand-placed.
5. **Destruction** follows real connections: the validator's connection graph becomes breakable constraints in a Blender rigid-body run, so models break into chunks that stay joined where the bricks were joined. Baked, then sampled on twos.
6. **Colors of damage** snap to the nearest real part color, never a tint.

## 5. Builds itself

The studio's signature move candidate (CREATIVE_DIRECTION.md thread 4) and the bridge between instruction sheets and motion.
- The Instructions Designer's step file (`steps.json`: step order, sub-assemblies, insertion direction per part) drives it.
- Each part arrives along its insertion direction over 2 to 3 exposures and seats with a one-plate settle, or pops into place in one exposure for the "set look". A click event per part goes to sound.
- Sub-assemblies arrive whole, the way a builder would add them.
- Disassembly runs the same steps in reverse.

## 6. Motion QC changes

Clay's motion QC table applies, with these changes. Computed from Blender data, never pixels.

| Check | Fail or warning | Starting threshold | Catches |
|---|---|---|---|
| **Rigid parts** | Fail | Any scale, shear, shape key or deform modifier on a part | Bending, stretching |
| **Joint limits** | Fail | Outside `joint_limits.json` | Impossible poses |
| **Rotations only at pivots** | Fail | A part moving relative to its parent other than about its pivot | Fake elbows and knees |
| **Feet on studs** | Fail | Planted foot off a stud position by more than 1 LDU | Sliding, floating feet |
| **Part interpenetration** | Fail | Overlap beyond the stud-in-tube allowance | Arms through bodies, hands through wheels |
| **Brick count stable** | Fail | Model part count or layout changes between frames unless a swap or attach is logged | Identity drift, popping |
| **FX legality** | Fail | Any FX part not a real part in a real color, or any non-brick volume or particle | Smooth CG fire and water |
| **Motion blur** | Fail | Motion blur enabled anywhere | Interpolated look |
| Held-frame change, cadence, foot skate, mouth timing | As in clay | As in clay | As in clay |
| Squash and stretch checks | Removed | n/a | Not allowed at all |

## 7. Build order (Animation TD), after Board Bar

1. Figure rig builder from catalog pivots; 12 base poses for one test figure.
2. Timing compiler on twos (shared engine); a first acting shot, no dialogue.
3. Face decal slots, Rhubarb mapper, blinks; a dialogue shot.
4. Stud solver and motion QC; a walk across a baseplate and picking up a brick.
5. Lens kit and camera rigs; a push-in with a steady-rig wobble.
6. Brick FX voxelizer; a splash and a puff of smoke.
7. Builds itself from a `steps.json`; a 4-second build.
8. Regression: every approved test shot recompiles to identical `exposures.json`.

## 8. Risks

- Rigid figures make acting harder; the ceiling is posing and face prints, not timing code.
- Joint limits guessed from images will be wrong; measure real figures.
- Brick FX at film scale can mean thousands of parts per frame; instancing is required (ENGINEERING.md).
