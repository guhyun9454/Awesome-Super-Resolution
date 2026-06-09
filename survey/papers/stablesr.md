# Exploiting Diffusion Prior for Real-World Image Super-Resolution

- **Model / Abbrev:** StableSR
- **Venue:** IJCV 2024 (2024)
- **Category:** Diffusion SR
- **Paper:** <https://arxiv.org/abs/2305.07015>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Real-world (blind) image super-resolution, where degradations are unknown and complex. The goal is to reuse the strong generative prior in large pre-trained text-to-image diffusion models for SR without the prohibitive cost of training a diffusion model from scratch and without sacrificing the prior's photorealism.

## Method
StableSR fine-tunes a frozen pre-trained Stable Diffusion (SD 2.1-base / 2.1-768v) for SR by attaching a lightweight, trainable time-aware encoder that injects features from the LR image into the frozen diffusion UNet via spatial feature transform (SFT)-style modulation. Because the synthesis backbone is left untouched, the generative prior is preserved and training cost is small. The time-aware design conditions the encoder on the diffusion timestep so guidance adapts across the denoising trajectory. To trade off realism vs. fidelity at inference, a Controllable Feature Wrapping (CFW) module (trained in the VAE/autoencoder latent decoding stage) fuses encoder features with the diffused features using a single adjustable scalar w. To handle arbitrary input resolutions despite SD's fixed training size, it uses a progressive patch-aggregation sampling strategy that blends overlapping patches with Gaussian weights during diffusion. Trained with the standard SD degradation/synthetic LR-HR pipeline.

## Key Contributions
- Time-aware encoder that adapts LR conditioning to the diffusion timestep while keeping the pre-trained SD synthesis model fully frozen, preserving the generative prior and minimizing training cost
- Controllable Feature Wrapping (CFW) module with a single scalar knob (w) to continuously balance fidelity (small w) versus perceptual quality/realism (large w) at inference time
- Progressive aggregation sampling that diffuses overlapping patches and fuses them (Gaussian weighting) to super-resolve images of arbitrary/large resolution beyond SD's fixed training size
- Demonstrates that frozen large-scale text-to-image diffusion priors can be repurposed for blind real-world SR, outperforming GAN-based and earlier diffusion SR methods on perceptual metrics

## Results
Uses ~200 DDPM steps (DDIM and a 4-step SD-Turbo variant added later). Evaluated on synthetic (DIV2K-Valid) and real-world benchmarks (RealSR, DRealSR), reporting strong perceptual scores (LPIPS, FID, and no-reference metrics such as CLIP-IQA/MUSIQ) and superior visual realism over GAN methods (e.g., Real-ESRGAN, BSRGAN) and prior diffusion SR (e.g., LDM); precise per-benchmark numbers are in the paper tables and were not enumerated on the abstract/repo pages fetched.

## Significance
StableSR was an influential early demonstration that a frozen Stable Diffusion prior plus a small trainable adapter beats bespoke GAN-based real-world SR on realism, establishing the diffusion-prior paradigm (later extended by DiffBIR, PASD, SeeSR, etc.) and the fidelity-quality controllability that subsequent work adopted.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*