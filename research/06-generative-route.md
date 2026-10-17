# Generative route for brick work (research, Oct 17, 2026)

Method: about 25 searches and 45 page fetches on Oct 17, 2026. Prices come from vendor pages where possible; reseller prices are marked. The Higgsfield catalog was read live from its own tool. [U] = unverified, vendor-only, or my inference. Numbers in brackets point to Sources.
Gaps: the shared search budget ran out before the platform-policy checks, so no LEGO-specific prompt filter was confirmed for any vendor. No hands-on bake-off was run. Seedance 2.5, Luma, Runway Gen-4.5, FLUX 3 Video and MiniMax prices are carried from the sibling clay report [46] and not rechecked. No independent brick-accuracy benchmark exists for image or video models.

## 1. Models, versions, prices (all as of Oct 17, 2026 unless dated)
Image (hosted)
- Gemini API [1][2]: Nano Banana 2.1 (gemini-nano-banana-2.1) $0.0336 per 1K image, $0.0504 2K, $0.113 4K; up to 10 object and 4 character references; thinking levels; image-search grounding. Nano Banana 2 (gemini-3.1-flash-image) $0.045 at 0.5K, $0.067 1K, $0.101 2K, $0.151 4K. Nano Banana 2 Lite $0.0336 at 1K only, weak at multi-reference. Nano Banana Pro (gemini-3-pro-image) $0.134 at 1K/2K, $0.24 at 4K; 6 objects, 5 characters, 3 style references. Batch is about half price. First-gen Nano Banana (gemini-2.5-flash-image) is legacy. Every image carries SynthID. Docs show text and image editing, no mask parameter (Higgsfield exposes an inpaint mask for NB2/2.1 [19]).
- OpenAI [6][7]: gpt-image-2 ($4 in / $15 out per 1M image tokens) and gpt-image-2.5 in two variants, flare (fast) and sunburst (precise edits); the 2.5 token table is garbled on the page. No official per-image table found. A reseller lists $0.025 to $0.03 per image (Oct 10) [8][U]. Higgsfield lists transparent background for 2.5 [19]. Org verification may be required.
- FLUX [9][10]: FLUX.2 pro/flex/max via API (Aug 2026 list: pro $0.03/MP create, max $0.07/MP [U, reseller blog]); klein 4B is Apache 2.0 and open; klein 9B and dev 32B are open but need a paid license to deploy commercially (outputs are usable). FLUX 3 Image is on Higgsfield with up to 10 references; BFL's calculator shows $0.048 per image without saying which size [10]. Kontext dev 12B is open.
- Midjourney V8.2: default since Jul 24, 2026; plans $10, $30, $60, $120 per month; --sref and personalization [11]. One source says no public API; Atlas Cloud sells V8.1 by API at $0.086 per 4-image job with no terms shown [12]. Treat API status as conflicting [U].
- Seedream 5.0 Pro: $0.045 up to 2.36 MP, $0.09 above; up to 10 references (Atlas Cloud, Aug 6, 2026) [13]. Ideogram 4.5 (up to 5 source images, typography) and Recraft V4.1 (utility mode for flat, front-facing product shots; vector mode for icons) are on Higgsfield [19]. Qwen-Image 2.1 (Sep 20, 2026): open 7B DiT, about 33 GB, Qwen Research License (non-commercial); Alibaba Pro/Turbo APIs since Oct 9 [14]. Z-Image (Tongyi) Apache 2.0 [15].
- Brick look: no head-to-head found. The toy look (glossy ABS, studs, tilt-shift) is easy for every model; structure is the weak point (section 2). Ranking for look alone is a guess: GPT Image, Nano Banana, Midjourney [U].
Video
- Veo 3.1 [3][4]: standard $0.40/s at 720p and 1080p, $0.60 at 4K; Fast $0.10, $0.12, $0.30; Lite $0.05 (720p) and $0.08 (1080p). Fast was $0.15 before an Apr 7 cut [5], so the clay report's figure is stale. 24 fps fixed, 4/6/8 s, native audio, up to 3 reference images (not Lite), first/last frame, extension by 7 s up to 20 times, files kept 2 days, SynthID, preview status. 1080p and references need 8 s.
- Kling 3.0 (Feb 2026): Krea API Aug 26 [16]: $0.1764/s std 720p, $0.2352 pro 1080p, $0.441 4K; audio adds 50%; start and end frame only, timed multi-prompt shots, 3 to 15 s. Identity references need Kling o1 at $0.1176/s. Reseller $0.075/s in March [U]. Runway Aleph 2.0 (Jul 29): video editing (restyle, replace objects, relight, keyframe edits) at $0.28/s [17]. Gen-4.5 about $0.126/s [16]. Seedance 2.0 from $0.0849/s [16]; Seedance 2.5 is live, price unconfirmed [13].
- Sora: announced shut down Mar 24, 2026, app off Apr 26, API off Sep 24 (one secondary source with an inconsistent date header) [18][U]. Do not plan on it. Open Wan ends at Wan 2.2; Wan 2.7 and 3.0 are API-only [15].
- Higgsfield is an aggregator. Live catalog [19]: video: Cinema Studio 3.0/3.5/4.0 (own), Kling 2.6/3.0/Turbo/Omni/Omni Edit/Motion Control, Seedance 1.5 Pro/2.0/2.5, Veo 3/3.1/3.1 Lite, Wan 2.6/2.7/3.0, MiniMax Hailuo/H3, FLUX 3 Video and Edit, Gemini Omni Flash 1.1, Grok Video 1.5; tools: Video Deflicker, FPS Boost (interpolation, avoid), upscalers, Sync Lipsync 3. Images: all Nano Banana versions, GPT Image 2/2.5, FLUX.2/3/Kontext, Seedream 4.5/5.0, Ideogram, Recraft, Soul 2.0. 3D: Meshy 6/7, Tripo H3.1/P2, Hunyuan3D v3/3.1, SAM 3 3D. Credit prices not read [U]. Its Scene Builder 3D tools expose Python and Blender export by name; untested [U].

## 2. Failure modes on bricks
- Direct evidence is thin. I found no public test of stud count, spacing or fused parts for image or video models. The list below mixes measured results on related tasks with inference.
- LEGO-Puzzles (Mar 2025; 1,100 elementary questions, 29 multimodal LLMs) [24]: best models trail humans by at least 20%; planning accuracy falls toward 0% as steps grow; asking for an image answer instead of multiple choice is worse, with zero accuracy at 3 steps. Read: models do not reliably understand brick assembly.
- Brick-Composer (Jun 2026) [23]: GPT-5.4 picked the right brick 43.9% of the time but placed it correctly step by step only 0.18% of the time; best fine-tuned system reached 14.3% step success.
- BrickBench (Oct 9, 2026) [22]: with a validator in the loop, frontier agents produce valid LDraw builds almost always, but texture requirements were met only 49% to 83% of the time, and humans identified the human-built model in 323 of 360 pairs. BrickGPT stayed valid but matched prompts poorly (VQA 0.024).
- BrickGPT paper [20]: general 3D generators often give designs that cannot be assembled from standard parts or collapse. Without its rollback and rejection sampling only 12.8% of outputs were stable.
- Expected pixel-model failures [U, inferred from the above]: wrong stud grid or count, plates and bricks of wrong thickness, fused or invented parts, floating pieces, minifigure hands and joints, parts that change between shots, melting seams in motion, glossy interpolated motion in place of stepped motion (Veo is fixed at 24 fps; the clay report saw 8 to 12 fps drift), invented "LEGO" wordmarks and garbled box text. An AI detector flagged one LEGO-branded ad as 95% AI but gave no reason [47]. The 2024 NINJAGO fan backlash was about ethics and borrowed iconography, not geometry [48].
- Text: GPT Image, Ideogram, Recraft and Nano Banana Pro are the best bets for text, but box art, labels and instruction pages should still be set in code.

## 3. Control methods
- Pattern A, restyle a Blender render: export beauty, depth, normal, edge and object-ID passes. Image tools: FLUX.1 Depth and Canny dev (full or LoRA; depth strength 0.6 to 0.9, canny 0.5 to 0.8); the page calls depth commercial-ok and canny non-commercial, which conflicts with the usual dev license, so verify [35][U]. Z-Image-Fun ControlNet Union 2.1 is used in a published workflow [34]. FLUX.2 leans on multi-reference editing; no verified FLUX.2 ControlNet [U]. Nano Banana and GPT Image can take the render as a reference and be told to keep layout, but expect drift of stud rows [U].
- Rule for bricks: restyle changes pixels, so it repaints studs. Keep the code-rendered brick pixels as the final layer and use restyle only for backgrounds, materials plates, lighting studies and VFX layers [U].
- Video-to-video, open: Wan 2.2 Fun VACE accepts depth, canny, pose, MLSD and trajectory control; official weights want about 60 GB VRAM, Kijai BF16/GGUF builds for less; 512 to 1024 px; 81 frames [33]. A ComfyUI "Blender to AI renderer" workflow feeds Blender passes into Wan 2.1 VACE 14B with Depth Anything 3, one start frame plus up to 3 references; normal passes not described [34].
- Video-to-video, hosted: Runway Aleph 2.0 [17], Kling 3.0 Omni Edit, FLUX 3 Video Edit, Gemini Omni Flash chat edits [19]. Whether any keeps a stud grid intact is untested [U].
- First/last frame: Veo 3.1 (Lite not listed), Kling 3.0, Seedance, Kling Omni Frames, Cinema Studio [3][16][19]. Good for camera moves; for pose A to pose B the model invents the in-between, which is where melting happens [U].
- Character consistency: NB 2.1 4 characters, NB Pro 5 characters plus 3 style refs, Seedream 5.0 Pro and FLUX 3 up to 10 refs, Veo 3.1 3 refs, Kling O3 elements [1][2][13][19]. A minifigure is a fixed object, so the strongest reference is a code render from your own part files.
- Brick-style LoRAs (card data from Hugging Face and Civitai, read Oct 17) [28] to [32]: merve/flux-lego-lora-dreambooth (FLUX.1 dev, license "other" = dev terms, data undisclosed); prithivMLmods/Flux-Lego-Ref-LoRA (FLUX.1 dev, openrail-m, 2024); nerijs lego-minifig-xl and lego-brickheadz-xl (SDXL, Apache 2.0, 2023, data unstated); Kontext-Style/LEGO_lora (Kontext dev, no license shown, training pairs generated by GPT-4o); Leg0W0rld Kontext LoRA (Civitai, Aug 6, 2025, dev non-commercial, 38 image pairs, photo to LEGO miniature keeping composition); Remade-AI/Lego (Wan2.1 14B T2V, Apache 2.0, about 1 minute of Lego-style video in 25 clips, trigger l3g0_5ty13). Download counts are tiny (under 700). Data provenance is unclear for nearly all [U].
- Cleaner option: train your own LoRA on your own Blender renders. Data is yours; license follows the base: FLUX.2 klein 4B, Z-Image, Wan 2.2 and Qwen-Image releases through Dec 2025 are Apache 2.0; Qwen-Image 2.1 is non-commercial [14][15]. Licenses attach to checkpoints, not families.
- Local runs: klein 4B, Z-Image, Wan 2.2 (14B or 5B), HunyuanVideo 1.5, LTX-2.5 (community license free under $10M revenue, per the clay report [U]) [46].

## 4. Reverse direction: image or text to real bricks
- BrickGPT (CMU, ICCV 2025; README calls it Best Paper) [20][21]: Llama-3.2-1B tuned on StableText2Brick (47,000+ structures, 28,000+ objects, 21 ShapeNet categories). 98.8% stable with rejection sampling and rollback. Limits: 20x20x20 grid, only 1-unit-tall axis-aligned bricks. Outputs txt, LDraw .ldr and PNG; texture and mesh-to-brick scripts included. MIT code and model; paper CC BY 4.0; Llama base is gated; Gurobi optional (else a connectivity check). The ShapeNet source license may limit commercial use [U].
- BrickBench and BrickAgent (arXiv Oct 2026) [22]: LLM agents with part search, connector placement and a validator beat the trained baselines. GPT-6 Astra: valid 1.00, VQA 0.954, $4.06 per assembly; GPT-6.1 Sol 0.945 at $0.87; Claude Opus 5.5 valid 0.99, VQA 0.923, $6.33. Without the environment, Astra was valid on 40% of prompts and a cheap model on under 1%. Code at kulits/BrickAgent (PyPI brickagent), license not stated. BrickNet-14B was valid on 26%.
- Newer papers: BrickAnything (May 25, v2 Aug 16, 2026; point cloud to bricks; no numbers in the abstract) [25]; ResemBrick (Aug 10, 2026; a few photos to a colored buildable model, zero floating or unstable bricks on held-out objects; no code mentioned) [26]; Brick-Composer (assembly from manual images) [23]; LegoACE (ACM, page unreadable) [U].
- Image to mesh [27]: Meshy 7 (Aug 12; vendor scores 81% proportion, 79.7% spatial, 59.8% surface detail), Tripo H3.1 (Mar 9), Hunyuan 3D 3.1 (hosted only), Pixal3D (MIT, open), TRELLIS.2 (MIT, Dec 16, 2025, 24 GB VRAM), SAM 3D via Higgsfield. All give smooth meshes with no part structure, so they need a voxelizer and brick packer; fine detail is the weak spot.
- Practical route [U]: pick a concept image -> have a coding LLM with a BrickAgent-style toolchain model it from parts -> validate. Use mesh-to-brick only as rough shape guidance.

## 5. Where gen models fit
- Good fit: concept and mood boards (about $0.03 to $0.13 per image); look-dev palettes and silhouettes; background plates shot out of focus; decal and print art (flat tiles, sticker sheets, torso prints; Recraft vector or utility); VFX layers such as smoke, water, fire, sky; upscaling (Topaz, Bytedance, SeedVR2 on Higgsfield or locally); scratch voice and sound (native audio in Veo and Kling; see the clay sound report [46]).
- Poor fit: stud-accurate close-ups, any shot where part count matters, build sequences and instruction pages (LEGO-Puzzles), long multi-shot character continuity, stepped brick motion, snapping-together animation, box and label text, anything that must look like a licensed set.
- Cost sense: 40 concepts on NB 2.1 at 1K is about $1.34; on NB Pro at 2K about $5.36.

## 6. Platform policies and legal (not legal advice)
- LEGO's own stance: a stated policy of not using generative AI to create LEGO content (Mar 15, 2024), while still exploring it [36]. LEGO Ideas bars AI images in submissions (reported Jul 13, 2026); BrickLink Designer Program allows them for sticker designs [37]. LEGO's site terms carry a "No AI" clause against using its site to train or run AI without written consent (Oct 8, 2026 article) [39].
- Fair Play page [38]: the LEGO logo must never appear on unofficial sites; the word LEGO may be used non-commercially as an adjective with the registered mark; the Basic Brick is a registered trademark in some countries; Minifigure is on the trademark list; the page says nothing about AI or films. Studio rule: use "brick-built", original parts and characters, no LEGO logo or wordmark, no lookalike minifigure marks. In the EU a 2010 court ruling reportedly refused a brick-shape trademark; not rechecked [U].
- Vendor filters: Google's Generative AI use policy (modified Dec 17, 2024) bars content violating others' IP rights but does not name trademarks [40]. OpenAI's usage policies (effective Oct 29, 2025) also cover IP [41]. Higgsfield's DTC ads image tool says it runs an IP check on products before queueing [19]. No evidence found that any major platform blocks or allows "LEGO" prompts by name [U]; the bake-off should log refusals.
- Litigation signal: Disney and Universal sued Midjourney in June 2025 over character outputs and training data, including secondary liability [42]; current status not checked [U]. Vendor tolerance could tighten.
- Training data: most LEGO-style LoRAs do not disclose data; assume some scraped imagery [U].
- Copyright: the US Copyright Office (Part 2, Jan 29, 2025) found prompts alone do not make the user an author; human-authored parts and selection can be protected, AI parts are excluded [43]. A code-built Blender film is human-authored and protectable; pure generated footage is weaker. This favors the hybrid.
- Disclosure: EU AI Act Article 50 has applied since Aug 2, 2026; providers must mark output in machine-readable form, deployers must disclose realistic synthetic content, grace to Dec 2, 2026 for systems already on the market, fines up to EUR 15M or 3% [44]. Gemini images and Veo video carry SynthID; C2PA metadata is often stripped by social platforms; keep sidecar manifests and log model version and prompt hash [45]. Brickfilm and LEGO fan communities react strongly to AI, so label gen-assisted assets [36][48].

## 7. Recommended hybrid pipeline and costs (estimates [U])
- Stage 0, concept (gen): 40 to 100 images on NB 2.1, GPT Image 2.5 or Midjourney; human picks palette, silhouettes, mood. Cost $2 to $15.
- Stage 1, spec (LLM): a coding agent turns picks into a part-level LDraw build with a BOM; iterate with the validator. BrickBench shows about $0.9 to $6.3 per assembly on top models. A 15-assembly film with 2 to 3 passes: about $30 to $300.
- Stage 2, build and render (code): real LDraw geometry in Blender, stepped animation on twos or threes, camera and lights in code. This is the picture the viewer sees for every brick.
- Stage 3, gen assist (optional): 12 background plates on NB Pro 2K ($0.134 each) with 4 tries: about $6; 20 decal tiles: $1 to $3; 20 VFX clips on Veo 3.1 Lite or Fast, 6 s at $0.30 to $0.60, 3 tries: $18 to $36; 6 restyle shots on Aleph 2.0, 5 s at $1.40, 3 tries: about $25. Gen layer for a 60 s short: roughly $30 to $80.
- Stage 4, composite and sound (code): ffmpeg or Blender compositor, hold timing in code, grain; voices and sound design per the clay sound report [46]. Keep a provenance log.
- Auto-check, build side: every part is an LDraw part ID, so stud count and BOM come from the file. Validate collisions, connectivity and stability. BrickBench's rig simulates each component in PyBullet and fails a part that moves over 3 LDU; it does not model clutch strength, so weak builds can pass [22]. Add your own rules (overhang limits, minimum connections) [U].
- Auto-check, image side: for any gen-touched frame, (1) edge-map similarity against the Blender edge pass (SSIM or chamfer) to catch drift; (2) FFT or Hough on a top-down crop to measure stud pitch against the expected pitch (LDraw: 20 LDU per stud, 24 per brick, 8 per plate, background knowledge); (3) palette check against the part colors in use; (4) OCR to flag any wordmark or garbled text; (5) a VLM yes/no question set judged from several views, as BrickBench does [22]; a Gemini Flash-class judge costs well under a cent per frame at $0.30 per 1M input tokens [1][U].
- Auto-check, video side: static regions must stay hash-identical across holds; optical flow against Blender motion vectors to catch melting; frame-difference test that holds really are holds [U].
- Policy gates: scan for LEGO logos and wordmarks; log model, version, prompt hash, SynthID or C2PA flags, and source assets per asset.

## 8. Open questions
- Does any model keep a stud grid through restyle or video-to-video? Run a bake-off: 5 subjects (2x4 brick close-up, 3x3 plate, minifigure-like figure, castle wall, car) on NB 2.1, NB Pro, GPT Image 2.5, FLUX.2, Seedream 5.0 Pro, Midjourney V8.2; then 4 Blender-driven video shots on Veo 3.1 Fast, Kling 3.0, Wan 2.2 VACE, Aleph 2.0. Score with the checks above.
- Unverified: official Midjourney API; any LEGO-specific vendor filter; Seedance 2.5 and Higgsfield credit prices; official gpt-image per-image prices; ShapeNet license effect on BrickGPT; LegoACE; current status of the Midjourney suit; FLUX depth license.

## Sources
[1] https://ai.google.dev/gemini-api/docs/pricing
[2] https://ai.google.dev/gemini-api/docs/image-generation
[3] https://ai.google.dev/gemini-api/docs/veo
[4] https://benchlm.ai/media-pricing/veo
[5] https://the-decoder.com/googles-veo-3-1-lite-cuts-video-generation-costs-by-more-than-half/
[6] https://developers.openai.com/api/docs/pricing
[7] https://developers.openai.com/api/docs/guides/image-generation
[8] https://docs.apiyi.com/en/live/2026-10/gpt-image-2-all-price-cut
[9] https://invideo.io/blog/flux-ai-image-generator/
[10] https://bfl.ai/pricing
[11] https://vidmuse.ai/blog/midjourney-8-2
[12] https://www.atlascloud.ai/models/midjourney/v8.1/text-to-image
[13] https://www.atlascloud.ai/blog/ai-updates/seedream-5-0-pro-price
[14] https://www.orcarouter.ai/blog/qwen-image-2-1
[15] https://invideo.io/blog/open-source-image-models-licenses/
[16] https://www.krea.ai/blog/kling-3-0-api-access-guide-pricing-code-examples-for-multi-shot-ai-video
[17] https://openrouter.ai/runway/aleph-2
[18] https://zilliz.com/ai-faq/what-is-the-sora-shutdown-timeline
[19] Higgsfield models_explore tool output, Oct 17, 2026 (no public URL)
[20] https://arxiv.org/html/2505.05469v3
[21] https://github.com/AvaLovelace1/BrickGPT and https://huggingface.co/cmu-gil/LegoGPT
[22] https://arxiv.org/pdf/2610.12452v1 and https://brickben.ch/
[23] https://arxiv.org/html/2606.05445
[24] https://arxiv.org/abs/2503.19990
[25] https://arxiv.org/abs/2605.26182
[26] https://arxiv.org/abs/2608.09597
[27] https://app.cinevva.com/guides/ai-3d-model-generation-timeline-2026
[28] https://huggingface.co/api/models?search=lego (and search=brick)
[29] https://huggingface.co/merve/flux-lego-lora-dreambooth
[30] https://huggingface.co/Remade-AI/Lego
[31] https://huggingface.co/Kontext-Style/LEGO_lora
[32] https://civitai.com/models/1843646
[33] https://www.stablediffusiontutorials.com/2025/09/wan2.2-vace-fun.html
[34] https://www.runcomfy.com/comfyui-workflows/blender-to-comfyui-ai-renderer-2-0-workflow-cinematic-video-output
[35] https://comfyui-wiki.com/en/tutorial/advanced/flux-controlnet-workflow-guide
[36] https://www.brickfanatics.com/lego-officially-comments-ninjago-ai-images
[37] https://www.brickfanatics.com/lego-ideas-prohibits-ai-images-in-submissions
[38] https://www.lego.com/en-us/legal/notices-and-policies/fair-play/
[39] https://www.thefashionlaw.com/what-legos-terms-reveal-about-brand-protection-in-the-ai-era/
[40] https://policies.google.com/terms/generative-ai/use-policy
[41] https://openai.com/policies/usage-policies/
[42] https://www.deeplearning.ai/the-batch/disney-and-universal-sue-midjourney-alleging-the-image-generator-violates-their-intellectual-property-rights
[43] https://ipwatchdog.com/2025/01/29/part-two-copyright-office-ai-report-says-creative-prompting-doesnt-constitute-authorship/
[44] https://archivemacropolo.org/analysis/eu-ai-act-article-50-watermarking-mandate
[45] https://tianpan.co/blog/2026-04-26-content-provenance-c2pa-synthid-audit-trail
[46] /home/claude/rader-ai/clay-motion/research/02-generative-video.md and 14-sound.md
[47] https://www.aiornot.com/tracker/image/ai-generated-lego-advertisement
[48] https://www.brickfanatics.com/legos-ai-lego-ninjago-art-test-was-a-disaster
