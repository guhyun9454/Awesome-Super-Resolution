# FiDeSR: High-Fidelity and Detail-Preserving One-Step Diffusion Super-Resolution

- **Model / Abbrev:** FiDeSR
- **Venue:** CVPR 2026 (2026)
- **Category:** One-step Diffusion
- **Paper:** <https://arxiv.org/abs/2603.02692>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
One-step diffusion SR methods (OSEDiff, PiSA-SR) struggle to simultaneously preserve fine details and ensure high-fidelity reconstruction, yielding suboptimal visual quality. FiDeSR targets closing this detail-vs-fidelity gap in real-world SR while keeping single-step efficiency.

## Method
Built on a frozen Stable Diffusion 2.1-base VAE+U-Net fine-tuned with rank-8 LoRA, distilled to a single inference step. Three components: (1) Detail-Aware Weighting (DAW) builds a difficulty map W=D⊙E from GT spatial operators (Sobel/Laplacian/variance) and error maps (L1 + LPIPS), used to spatially reweight the reconstruction and Classifier Score Distillation losses so training emphasizes high-error/high-detail regions; (2) Latent Residual Refinement Block (LRRB), an RRDB-style network in latent space that predicts a correction Δr to the U-Net's coarse residual (r' = r + Δr), i.e. residual-in-residual noise refinement; (3) Latent Frequency Injection Module (LFIM), an FFT/Butterworth-filter decomposition into low/high-frequency bands gated spatially (detail) and per-channel (frequency-energy ratio), applied post-diffusion pre-VAE-decode at inference with no retraining for flexible enhancement control.

## Key Contributions
- Detail-Aware Weighting (DAW): spatial-operator + error-map difficulty weighting applied to reconstruction and CSD losses to focus learning on hard detail regions
- Latent Residual Refinement Block (LRRB): RRDB-based latent-space module that corrects the diffusion noise/residual prediction (residual-in-residual refinement) for better fine-detail recovery
- Latent Frequency Injection Module (LFIM): training-free, FFT/Butterworth low/high-frequency adaptive enhancers with spatial and channel gating for controllable inference-time refinement
- SOTA among one-step diffusion SR with strong gains in both full-reference and no-reference metrics at ~0.078s/image (128x128) on H100

## Results
One-step. On DRealSR: PSNR 28.90 / SSIM 0.791 / LPIPS 0.284 / MANIQA 0.624 / FID 127.97, beating PiSA-SR-1s (28.32/0.780/0.296/0.616/130.48), OSEDiff, SeeSR-50s and StableSR-200s across the board. RealSR: 26.02/0.746/0.263/0.668 vs PiSA-SR 25.50/0.742/0.267/0.655. DIV2K-val: 24.33/0.625/0.268/0.638 vs PiSA-SR 23.87/0.606/0.282. ~1.29B params (vs PiSA-SR 1.30B); single step vs 20-200 for multi-step competitors.

## Significance
Shows that combining error/detail-aware loss reweighting, a latent residual-refinement correction of the diffusion noise, and a training-free frequency-injection enhancer lets a one-step diffusion model improve fidelity (PSNR/SSIM) AND perceptual/detail metrics (LPIPS/FID/MANIQA) at once, setting a new one-step SR SOTA while preserving real-time-class efficiency.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*