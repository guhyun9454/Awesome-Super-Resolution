# CubeComposer: Spatio-Temporal Autoregressive 4K 360° Video Generation from Perspective Video

- **Model / Abbrev:** CubeComposer
- **Venue:** CVPR 2026 (2026)
- **Category:** Video SR
- **Paper:** <https://arxiv.org/abs/2603.04291>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Generating immersive 360° panoramic videos for VR from a standard perspective video. Prior methods (e.g., Argus) are capped at ~1K native resolution and rely on suboptimal post-hoc upscaling; producing native 4K 360° video in one diffusion pass is infeasible due to prohibitive peak memory and the seam/continuity problems of panoramic representations.

## Method
Built on the Wan 2.2 5B video diffusion transformer (DiT), trained with a flow-matching / velocity-prediction objective. The panorama is decomposed into a cubemap of six faces (Front/Right/Back/Left/Up/Down, each R×R) and the video is split into L temporal windows. Rather than denoising the whole 360° video at once, CubeComposer autoregressively generates small spatio-temporal blocks (one cube face × one time window at a time) in a coverage-guided order (faces sorted by descending spatial coverage of the perspective input within each window), drastically cutting peak memory and enabling native 4K. Generation is conditioned on already-produced faces/windows via a context-management mechanism, supporting both global and per-face text prompts.

## Key Contributions
- Spatio-temporal autoregressive strategy that orchestrates 360° generation across cube faces and time windows, generating small blocks sequentially to slash peak memory and reach native 4K
- Cube face context management with a sparse context attention design: the current generation sequence (length G) does full self-attention and context (length C) attends fully to generation but only sparsely to itself via a diagonal-banded local mask of bandwidth K, cutting context cost from O(C^2) to O(C*K)
- Continuity-aware designs to remove cubemap boundary seams: cube-aware positional encoding remapped to the flattened cubemap topology, topology-aligned padding (adjacent-face strips with proper rotations/flips), and overlap blending via weighted averaging
- 4K360Vid dataset: 11,832 high-quality 4K 360° video clips with both global and frame-wise captions; training also uses the high-res ODV360 subset
- Simulated-autoregression training on ground-truth 360° videos by sampling windows/faces, with global and face-wise prompt support

## Results
Native 4K (3840×1920) equirectangular output. On 4K360Vid/ODV360 benchmarks (VBench-style metrics), CubeComposer at 4K: LPIPS 0.3831, CLIP 0.9111, FID 130.92, FVD 2.22, Aesthetic 0.4051, Imaging 0.5618, Overall Consistency 0.1769. At 2K: LPIPS 0.3696, CLIP 0.9234, FID 119.10, FVD 3.90. Beats Argus (1K): LPIPS 0.4074, CLIP 0.8858, FID 141.15, FVD 4.08 — improving across native resolution, fidelity, and temporal coherence (notably low FVD 2.22 at 4K).

## Significance
First framework to natively synthesize 4K 360° video (vs. prior ≤1K + upscaling), made tractable by autoregressive cube-face/time-window block generation that bounds memory while preserving cross-face and temporal coherence. It makes high-resolution panoramic video generation practical for real VR/immersive applications and offers a reusable recipe (sparse context attention + cube-aware PE/padding/blending) for seam-free, memory-efficient panoramic synthesis. Code released by TencentARC.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*