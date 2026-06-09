# Energy-oriented Diffusion Bridge for Image Restoration with Foundational Diffusion Models

- **Model / Abbrev:** E-Bridge
- **Venue:** arXiv 2026 (2026)
- **Category:** Diffusion SR
- **Paper:** <https://arxiv.org/abs/2604.10983>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Diffusion bridges directly transport between degraded and clean image distributions but their trajectories carry high transport energy, requiring many sampling steps, and adapting them to leverage large pretrained foundational diffusion models is non-trivial. E-Bridge targets few-step, perceptually high-quality image restoration that exploits a foundation diffusion prior.

## Method
E-Bridge builds an energy-oriented diffusion bridge on top of the large-scale FLUX-dev foundation diffusion model (fine-tuned ~10k iterations). The core idea is to approximate low-cost manifold geodesic trajectories: instead of running a full bridge from a degraded image, it defines a bridge process over a shortened time horizon whose reverse process starts from an entropy-regularized point that mixes the degraded image with Gaussian noise, which theoretically reduces the required trajectory transport energy. To sample efficiently it adopts a consistency-model formulation, learning a single-step mapping function optimized with a continuous-time consistency objective tailored to the bridge trajectory. The trajectory length becomes a tunable, task-adaptive knob balancing information preservation versus generative power for different degradation severities.

## Key Contributions
- Energy-oriented diffusion bridge that approximates low-cost manifold geodesic trajectories, reducing transport energy via a shortened time horizon
- Entropy-regularized reverse-process starting point mixing the degraded image with Gaussian noise (rather than starting from the raw degraded image)
- Continuous-time consistency objective adapted to the bridge for single/few-step mapping, built on the FLUX-dev foundational diffusion model
- Task-adaptive tunable trajectory length that trades off fidelity vs. generative power per degradation type
- Demonstrated across diverse restoration tasks (SR, denoising, raindrop removal, low-light enhancement, demoiréing) at only 5-10 NFE

## Results
Backbone FLUX-dev fine-tuned ~10k iters; runs at only 5-10 NFE vs. 20-100 for competitors. Reported sample results at 10 NFE: super-resolution PSNR 21.28 / LPIPS 0.346 / MUSIQ 70.87 / NIQE 3.84; denoising (Gaussian sigma=50) PSNR 26.06 / LPIPS 0.184 / MUSIQ 69.74 / NIQE 3.51. SR trained on 3,450 DIV2K+Flickr2K images with Real-ESRGAN degradation, tested on 100 DIV2K val images; also evaluated on Raindrop (861/58), LOLv1 low-light, and UHDM demoiréing (4,500/500). Baselines: DiffBIR, AutoDIR, UniDB, IRBridge, MaRS, UniDB++, DA-CLIP, DiffUIR, AdaIR. Authors note it deliberately prioritizes perceptual realism (LPIPS/FID/NIQE/MUSIQ) over raw PSNR, achieving SOTA/highly competitive perceptual and distributional metrics.

## Significance
It shows how a massive foundational text-to-image diffusion model (FLUX-dev) can be turned into an efficient few-step (5-10 NFE) restoration bridge, recasting bridge trajectory length as an energy-minimization / geodesic problem and as a controllable task-adaptive knob. This advances the trend of leveraging foundation diffusion priors for perceptually-realistic, low-step image restoration.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*