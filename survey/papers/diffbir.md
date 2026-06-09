# DiffBIR: Towards Blind Image Restoration with Generative Diffusion Prior

- **Model / Abbrev:** DiffBIR
- **Venue:** ECCV 2024 (2024)
- **Category:** Diffusion / Real-world SR
- **Paper:** <https://arxiv.org/abs/2308.15070>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Blind image restoration must recover high-quality images from inputs degraded by unknown, complex real-world degradations. Prior task-specific methods do not generalize across degradation types, and naively applying generative diffusion priors tends to introduce noise-driven artifacts and hurt fidelity.

## Method
DiffBIR is a unified pipeline that decouples blind restoration into two stages. Stage 1 (degradation removal) applies a task-specific restoration module (e.g., SwinIR-based regressor; BSRNet/SwinIR-style backbones) to strip image-independent degradations (blur, noise, downsampling, compression) and produce a clean but possibly over-smoothed condition image. Stage 2 (information regeneration) feeds that condition into IRControlNet, a ControlNet-style adapter attached to a frozen pretrained latent diffusion model (Stable Diffusion 2.1) that hallucinates realistic lost detail. Critically, IRControlNet is trained on the cleaned stage-1 outputs (condition images free of noisy content) rather than raw degraded images, giving stable, robust conditioning. The diffusion prior is frozen; only the IRControlNet branch is trained, decoupling generation from the restoration backbone.

## Key Contributions
- Unified two-stage framework (degradation removal then information regeneration) that handles blind image super-resolution, blind face restoration, and blind denoising in one pipeline
- IRControlNet: a ControlNet adapter conditioned on noise-free stage-1 restoration outputs, leveraging a frozen Stable Diffusion latent diffusion prior for realistic detail synthesis with stable training
- Region-adaptive restoration guidance: a training-free, inference-time mechanism that modifies the denoising trajectory (gradient guidance toward the condition) to balance fidelity vs. realism without retraining
- Tunable guidance scale letting users trade off realness against fidelity at inference time
- Strong generalization to real-world degradations, validated on synthetic and real datasets across three restoration tasks

## Results
Reports state-of-the-art perceptual quality on real-world blind SR, blind face restoration, and blind denoising benchmarks (synthetic and real). Uses the Stable Diffusion 2.1 prior with standard multi-step DDPM/spaced sampling (commonly ~50 steps); excels on no-reference perceptual metrics (e.g., MANIQA, MUSIQ, CLIP-IQA, NIQE) and LPIPS/FID, trading some pixel-fidelity (PSNR/SSIM) for markedly higher realism versus regression-based methods. Exact per-benchmark numbers not in the fetched abstract.

## Significance
DiffBIR showed that decoupling degradation removal from generative regeneration — and conditioning a frozen Stable Diffusion prior on cleaned images rather than raw degraded inputs — yields a single robust pipeline that generalizes across multiple blind restoration tasks. It became a widely cited baseline for diffusion-prior-based real-world SR and restoration.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*