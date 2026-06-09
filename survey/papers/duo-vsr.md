# DUO-VSR: Dual-Stream Distillation for One-Step Video Super-Resolution

- **Model / Abbrev:** DUO-VSR
- **Venue:** CVPR 2026 (2026)
- **Category:** One-step Diffusion (Video SR)
- **Paper:** <https://arxiv.org/abs/2603.22271>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Diffusion-based video super-resolution (VSR) produces high-quality results but is prohibitively slow, requiring ~50 sampling steps per clip. Distilling such a multi-step diffusion VSR model into a single forward step typically sacrifices detail richness and temporal consistency, and pure distribution-matching distillation (DMD) tends to produce over-smoothed outputs. DUO-VSR targets fast (one-step), high-fidelity, temporally coherent VSR.

## Method
DUO-VSR distills a 50-step, 1.3B-parameter video DiT (latent diffusion transformer adapted from an internal text-to-video model) into a one-step student via a three-stage pipeline. Stage 1 (Progressive Guided Distillation Initialization) stabilizes training by first CFG-distilling the conditional/unconditional branches, then progressively halving denoising steps from 64 down to 1 with trajectory-preserving L2 matching to the teacher. Stage 2 (Dual-Stream Distillation) jointly trains two complementary streams: a DMD stream (frozen real score model + a trainable fake score model, score-distribution matching with normalized gradient) and a Real-Fake Score Feature GAN (RFS-GAN) stream that reuses both score models as discriminator backbones with a hinge GAN + feature-matching objective, providing adversarial supervision that recovers high-frequency detail the DMD stream alone over-smooths. Updates alternate one student step per three auxiliary (score/discriminator) steps. Stage 3 (Preference-Guided Refinement) applies Direct Preference Optimization on synthetic preference pairs to align outputs with perceptual quality.

## Key Contributions
- Dual-Stream Distillation: unifies distribution matching distillation (DMD) with an adversarial RFS-GAN stream that reuses the diffusion score models as discriminator backbones, combining the stability of DMD with the detail/sharpness of GAN supervision for one-step VSR
- Progressive Guided Distillation Initialization: a CFG-distillation + step-halving (64->1) warm-up that stabilizes subsequent one-step distillation via trajectory preservation
- Preference-Guided Refinement via DPO on synthetic preference pairs to further align the one-step student with human perceptual preference
- Constructs AIGC60, a 60-video benchmark for super-resolving AI-generated video content, alongside evaluation on synthetic and real-world sets
- Single-step inference: ~50x faster than SeedVR-7B and ~90x faster than 50-step diffusion VSR, while improving perceptual and temporal-consistency metrics

## Results
Single forward step, 1.3B generator. Reported quality: YouHQ40 PSNR 22.90 / SSIM 0.691 / LPIPS 0.315 / NIQE 3.59 / MUSIQ 66.91 / CLIP-IQA 0.5459 / DOVER 81.47; SPMCS PSNR 24.94 / SSIM 0.726 / LPIPS 0.259; UDM10 PSNR 22.96 / SSIM 0.674 / LPIPS 0.289; real-world VideoLQ NIQE 4.42 / MUSIQ 63.68 / DOVER 88.15. Inference 11.3s for a 21-frame 1920x1080 video on a single GPU (~50x faster than SeedVR-7B, ~90x faster than 50-step methods, ~5x faster than competing one-step approaches). Trained on 830k RealBasicVSR-degraded paired samples. Human GSB study: preferred over all baselines (competitors at -25.5% to -39.8%). Compared against SeedVR2-7B, DOVE, DLoRAL, STAR, UAV, MGLD, VEnhancer, RealViformer, FlashVSR, UltraVSR.

## Significance
Shows that combining distribution-matching and adversarial (GAN) supervision in a single distillation framework resolves the over-smoothing of DMD-only one-step models, pushing diffusion VSR to genuinely real-time-friendly latencies (11.3s for a 21-frame 1080p clip on one GPU) without quality loss. It also extends one-step VSR to the emerging problem of restoring AI-generated video.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*