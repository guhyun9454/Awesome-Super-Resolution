# ESRGAN: Enhanced Super-Resolution Generative Adversarial Networks

- **Model / Abbrev:** ESRGAN
- **Venue:** ECCVW 2018 (2018)
- **Category:** GAN / Perceptual SR
- **Paper:** <https://arxiv.org/abs/1809.00219>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Single-image super-resolution at 4x where prior GAN-based methods (SRGAN) produce sharp but artifact-laden, unnatural textures. The goal is to push perceptual quality and texture realism higher while suppressing the unpleasant hallucinated artifacts characteristic of SRGAN.

## Method
ESRGAN improves SRGAN along three axes. (1) Architecture: it replaces SRGAN's residual blocks with the Residual-in-Residual Dense Block (RRDB), a deeper unit combining multi-level residual connections and dense connections, and removes all Batch Normalization layers (which introduce artifacts and hurt generalization in SR). Residual scaling and smaller initialization stabilize training of the deeper net. (2) Adversarial loss: it adopts the Relativistic average GAN (RaGAN), so the discriminator estimates whether a real image is relatively more realistic than a fake one rather than predicting absolute realness, recovering sharper edges/textures. (3) Perceptual loss: features are taken before the VGG activation rather than after, giving denser supervision and better brightness/texture consistency. The total generator loss combines this perceptual loss, the relativistic adversarial loss, and an L1 content loss.

## Key Contributions
- Residual-in-Residual Dense Block (RRDB) generator with Batch Normalization removed, enabling a deeper, higher-capacity, artifact-reduced model
- Relativistic average discriminator (RaGAN) that judges relative rather than absolute realness, improving texture sharpness and training stability
- Improved perceptual loss using VGG features before activation for stronger texture/brightness supervision
- Network interpolation: a post-hoc strategy that linearly interpolates parameters of a PSNR-oriented model and the GAN model to continuously trade off fidelity (PSNR) vs. perceptual quality without retraining or introducing artifacts
- Won 1st place (Region 3, best perceptual index) in the PIRM2018-SR Challenge

## Results
Won 1st place in the PIRM2018-SR Challenge (Region 3) with the best perceptual index. Produces consistently more realistic and natural textures with fewer artifacts than SRGAN, achieving better perceptual quality at 4x SR. The network-interpolation trick yields a smooth fidelity-vs-perception trade-off, removing noise while preserving texture. (The paper emphasizes perceptual-index / visual-quality wins over raw PSNR/SSIM, since GAN-based SR intentionally trades PSNR for perceptual quality.)

## Significance
ESRGAN became the de-facto baseline and backbone for perceptual super-resolution; its BN-free RRDB generator and pre-activation perceptual loss are reused across countless later SR, restoration, and Real-ESRGAN-style real-world models. It established practical recipes (network interpolation, relativistic discriminator) for controlling the perception-distortion trade-off.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*