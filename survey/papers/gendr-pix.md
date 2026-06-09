# Eliminating VAE for Fast and High-Resolution Generative Detail Restoration

- **Model / Abbrev:** GenDR-Pix
- **Venue:** ICLR 2026 (2026)
- **Category:** One-step Diffusion
- **Paper:** <https://arxiv.org/abs/2602.10630>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Diffusion-based super-resolution is slow and memory-hungry. Even one-step distilled models (GenDR) must tile-process high-resolution (e.g. 4K) images because of memory limits. The authors identify the VAE encoder/decoder, not the UNet denoiser, as the dominant bottleneck for speed and memory at high resolution.

## Method
GenDR-Pix converts a latent-space one-step diffusion SR model (GenDR, built on SD 2.1 UNet with a vae-kl-f8-d16) into a pixel-space model by replacing the VAE with cheap pixel-(un)shuffle operations. A two-stage adversarial distillation progressively removes the VAE components: Stage I swaps the VAE encoder for x8 pixel-unshuffle (using the frozen GenDR UNet as a discriminator with a latent-matching loss); Stage II swaps the VAE decoder for x8 pixel-shuffle (using the Stage I model GenDR-Adc as discriminator). Generative features from the prior-stage model guide the adversarial discrimination, random padding augments features to prevent discriminator collapse, and a masked Fourier-space loss penalizes amplitude outliers. Inference adds padding-based self-ensemble combined with classifier-free guidance.

## Key Contributions
- Identifies the VAE (not the UNet) as the principal speed/memory bottleneck for high-resolution one-step diffusion SR
- Pixel-space reformulation: replaces VAE encoder/decoder with x8 pixel-unshuffle/pixel-shuffle, eliminating the latent autoencoder entirely
- Two-stage adversarial distillation that uses the previous-stage generative model as the discriminator (latent-matching, then GenDR-Adc) to progressively strip the VAE
- Training tricks: random padding feature augmentation against discriminator collapse and a masked Fourier-space loss penalizing amplitude outliers
- Padding-based self-ensemble combined with classifier-free guidance at inference

## Results
One-step inference, 866M params, SD 2.1 backbone, trained on ~20M LQ-HQ pairs (RealESRGAN + APISR degradation), supports x2/x3/x4. vs GenDR: 2.8x faster, ~60% less memory; restores a full 4K image without tiling in ~1s using ~6GB (paper also reports 3.58s / 15.10GB for full 4K on one config). RealSet80 x4: MUSIQ 69.92, CLIPIQA 0.7190, NIQE 4.10, 32ms on A100. ImageNet-Test x4: PSNR 25.49, SSIM 0.7286, LPIPS 0.2252, NIQE 4.19, MUSIQ 72.85, CLIPIQA 0.7168. RealSR x4: PSNR 25.03, LPIPS 0.2702, MUSIQ 66.29. Reports negligible quality loss vs GenDR while outperforming other one-step diffusion SR methods.

## Significance
Shows that for one-step diffusion SR, the VAE rather than the denoiser is the practical efficiency bottleneck, and that a pixel-space model distilled from a latent teacher can match latent-model quality while enabling tiling-free 4K restoration in around a second on modest memory. This reframes the efficiency frontier of generative restoration and makes real-time high-resolution diffusion SR practical.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*