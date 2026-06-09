# Image Super-Resolution via Iterative Refinement

- **Model / Abbrev:** SR3
- **Venue:** TPAMI 2022 (2022)
- **Category:** Diffusion SR
- **Paper:** <https://arxiv.org/abs/2104.07636>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Single-image super-resolution where regression-based methods produce blurry, over-smoothed outputs by averaging plausible high-res solutions, and GAN-based methods suffer from unstable training and mode collapse. SR3 seeks photo-realistic upsampling that captures the multimodal conditional distribution of high-res images given a low-res input.

## Method
SR3 adapts Denoising Diffusion Probabilistic Models (DDPM) to conditional generation for super-resolution. A U-Net denoising network is conditioned on the low-resolution image (bicubically upsampled to the target size and channel-concatenated with the noisy target) and trained to predict the noise added at each step. Generation starts from pure Gaussian noise and iteratively refines it over many reverse-diffusion steps (e.g., ~100-2000), each step a conditioned Gaussian denoising. Training uses a simple denoising objective (L1/L2 regression on the added noise) across a continuum of noise levels, conditioning the network on the noise level rather than a discrete timestep.

## Key Contributions
- First to adapt DDPMs to conditional image-to-image super-resolution, conditioning a U-Net denoiser on the low-res input via channel concatenation
- Iterative stochastic refinement from Gaussian noise yields sharp, photo-realistic outputs avoiding the blur of regression and instability of GANs
- Trains with a single simple noise-prediction (denoising) loss across continuous noise levels; conditions on noise level (gamma) instead of discrete step index
- Cascaded generation pipeline: chain multiple SR3 models (and an unconditional generator) for high-resolution synthesis, e.g. 64x64 to 256x256 to 1024x1024
- Demonstrates strong human-evaluation fool rates approaching the ideal 50%, substantially beating GAN baselines

## Results
On 16x16 to 128x128 CelebA-HQ face SR (8x), achieves a human fool rate near 50% (vs ~34% for the PULSE/FSRGAN GAN baselines), indicating near-indistinguishable-from-real outputs. On natural-image SR, a two-stage cascade (64x64 -> 256x256) on ImageNet reaches FID ~11.3, competitive with strong generative baselines. Note: SR3 deliberately favors perceptual realism, so its pixel PSNR/SSIM can be lower than regression methods despite better visual fidelity. Inference requires many iterative refinement steps (slow vs single-pass models).

## Significance
SR3 was a foundational demonstration that diffusion models can outperform GANs and regression methods for photo-realistic super-resolution, helping launch the wave of diffusion-based image restoration. Its conditioning-by-concatenation and cascaded-refinement design directly influenced later cascaded diffusion and text-to-image systems (e.g., Imagen).

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*