# LiveMoments: Reselected Key Photo Restoration in Live Photos via Reference-guided Diffusion

- **Model / Abbrev:** LiveMoments
- **Venue:** ICLR 2026 (2026)
- **Category:** Reference-based SR
- **Paper:** <https://arxiv.org/abs/2604.12286>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
In Live Photos, the camera ISP delivers a high-quality key photo plus a lower-quality video clip. When users reselect an alternative video frame as the key photo (for a better expression/timing), that frame shows visible quality degradation versus the original ISP-processed key photo. LiveMoments restores the reselected frame using the original high-quality key photo as a reference.

## Method
A two-branch reference-guided latent diffusion model built on Stable Diffusion 3 Medium (both branches initialized from SD3-medium, frozen VAE). A ReferenceNet mirrors the denoising backbone and extracts structure/texture features from the original key photo; a RestorationNet restores the reselected frame, fusing reference info via cross-attention (reference K/V concatenated with restoration-branch queries). A unified Motion Alignment module handles content displacement between the two moments: at the latent level, RAFT optical flow is encoded by a lightweight Motion Encoder and added as a bias to reference key features in cross-attention; at the image level, a Patch Correspondence Retrieval (PCR) strategy shifts patch coordinates to fetch aligned reference regions during tiled 4K inference. Training uses a bridge/flow-matching objective z_t = alpha(t)z_Hs + beta(t)z_Ls + sigma(t)*eps that perturbs with only ~20% Gaussian noise to preserve low-frequency content, restricting the trajectory to t in [0,0.2] and enabling fast sampling.

## Key Contributions
- Defines and tackles the novel task of restoring reselected key photos in Live Photos, where a paired high-quality reference frame is available but spatially misaligned by motion
- Two-branch ReferenceNet + RestorationNet design on SD3-medium that transfers reference texture/structure via concatenated cross-attention K/V
- Unified Motion Alignment module operating at both latent level (RAFT flow -> Motion Encoder -> additive cross-attention bias) and image level (Patch Correspondence Retrieval for tiled 4K inference)
- Bridge-matching forward process with only ~20% noise perturbation over t in [0,0.2], enabling 6-step inference while preserving low-frequency detail
- New benchmarks: SynLive260 (synthetic), vivoLive144 (vivo X200 Pro), iPhoneLive90 (iPhone 15 Pro), plus a 75,400-pair training set from DL3DV and internet videos

## Results
On SynLive260 (full-reference): PSNR 31.65, SSIM 0.899, LPIPS 0.083, FID 4.00, beating CoSeR (27.60 / 0.814 / 0.244 / 13.92). On real vivoLive144 and iPhoneLive90 it leads on no-/relative-reference metrics (NIQE, MUSIQ, CLIPIQA, MANIQA) and CLIP-Q/DINO-Q identity preservation (e.g. vivo CLIP-Q 0.9805, DINO-Q 0.9629). 6 sampling steps; ~4,268M total params (incl. VAE, RAFT, both nets); 1.89 s per 1024x1024 patch and ~40 s per 4K image on an H20 GPU; trained 48 h on 2x H20, lr 5e-5, batch 8. Human study: 69.62% preference over three baselines.

## Significance
First framework to exploit the unique paired-reference structure of Live Photos for restoration, turning the high-quality key photo into motion-aware guidance and combining reference-based SR with fast bridge-matching diffusion. It pushes practical, ISP-aware, 4K-capable reference restoration on real mobile-camera data and sets up new benchmarks for the task.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*