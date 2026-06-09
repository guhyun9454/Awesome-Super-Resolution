# Face2Scene: Using Facial Degradation as an Oracle for Diffusion-Based Scene Restoration

- **Model / Abbrev:** Face2Scene
- **Venue:** CVPR 2026 (2026)
- **Category:** Reference-based SR
- **Paper:** <https://arxiv.org/abs/2603.16570>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Blind restoration of full real-world scenes (face + body + background) under unknown, spatially-mixed degradation (noise, blur, compression). The key insight: in person-centric photos a high-quality identity reference exists for the face but not the rest of the scene, so the face can serve as a calibrated "probe" of the unknown degradation operator that otherwise must be blindly estimated for the whole image.

## Method
Two-stage framework. Stage 1: a reference-based face restoration model (FaceMe, at 512x512) reconstructs HQ facial detail from the degraded face crop plus one or more identity references. Stage 2: a face-derived degradation extractor (FaDeX, a lightweight conv encoder over the concatenated HQ-LQ face pair) infers a degradation code, which a mapping network (MapNet) converts into 21 multi-scale degradation-aware tokens (4x4 + 2x2 + 1x1 grids via dual-branch attention). These tokens condition a one-step SD-Turbo diffusion backbone (with LoRA on VAE encoder rank 16 and U-Net rank 32) to restore the entire image in a single diffusion step. The face thus acts as a "perceptual oracle" that calibrates degradation estimation for the rest of the scene where no reference is available.

## Key Contributions
- Reframes scene restoration around a face-as-oracle paradigm: uses the HQ-LQ residual from reference-based face restoration to estimate the global degradation operator, sidestepping blind degradation estimation for the full image
- FaDeX: a contrastive degradation extractor trained with a momentum encoder so face pairs sharing the same degradation operator attract in embedding space
- MapNet: maps the degradation code into 21 multi-scale tokens via overlapping patch embedding and a dual-branch attention with a learnable scalar lambda
- Single-step diffusion scene restoration on SD-Turbo + LoRA, conditioned on the face-derived tokens (CFG 1.10), avoiding multi-step sampling for the scene
- New curated datasets/benchmark (InScene): ~11.3K synthetic images (905 IDs, CelebRef-HQ + InfiniteYou with anti-blur LoRA, ArcFace>=0.55 filtering), ~16.5K real training images, plus a 100-image Samsung S25 Edge mobile test set

## Results
On the InScene real validation set vs S3Diff: LPIPS 0.2502 vs 0.5149, DISTS 0.1178 vs 0.2231, PSNR 22.90 vs 17.14 dB, SSIM 0.6197 vs 0.4894, MUSIQ 75.37 vs 73.82, CLIP-IQA 0.7015 vs 0.6734 (FID 42.21 vs S3Diff's slightly better 38.64). Inference 3.3s per 1024x1024 image (Stage I face restoration 2.0s + Stage II 1.3s); scene restoration uses 1 diffusion step. Losses: L2 (lambda 2.0) + LPIPS (5.0) + DINO-discriminator GAN (0.5), plus contrastive FaDeX loss.

## Significance
Introduces a practical, reference-grounded route to blind full-scene restoration that exploits the asymmetry of person photos (face has a reference, scene does not), turning identity references into a degradation oracle. Demonstrates that one-step diffusion conditioned on this oracle can beat strong blind one-step baselines on real data while remaining fast (3.3s/1024px), making it deployable for mobile photo enhancement.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*