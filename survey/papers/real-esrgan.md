# Real-ESRGAN: Training Real-World Blind Super-Resolution with Pure Synthetic Data

- **Model / Abbrev:** Real-ESRGAN
- **Venue:** ICCVW 2021 (2021)
- **Category:** Real-world SR
- **Paper:** <https://arxiv.org/abs/2107.10833>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Real-world low-resolution images suffer from complex, unknown (blind) degradations that the classical bicubic-downsampling assumption fails to model, so SR networks trained on synthetic bicubic pairs generalize poorly to real photos. Collecting paired real LR-HR data is impractical, motivating a purely synthetic but realistic degradation pipeline.

## Method
Real-ESRGAN extends ESRGAN with a "high-order" (specifically second-order) degradation model that repeats a classical degradation sequence twice to better approximate real-world image corruption. Each order applies randomized blur (isotropic/anisotropic Gaussian and generalized/plateau kernels), downsampling (area/bilinear/bicubic), noise (Gaussian and Poisson, gray/color), and JPEG compression; sinc filters are added to synthesize common ringing and overshoot artifacts. The generator is the ESRGAN RRDB backbone, with pixel-unshuffle used to handle x2 and x1 scales while keeping computation on a lower-resolution feature space. Training is GAN-based with a combined L1 pixel loss, perceptual (VGG feature) loss, and adversarial loss.

## Key Contributions
- Second-order/high-order degradation modeling that composes two rounds of blur-downsample-noise-JPEG to synthesize realistic, diverse training pairs from pure HR data (no real LR-HR pairs needed)
- Explicit modeling of ringing and overshoot artifacts via sinc filters in the degradation pipeline
- A U-Net discriminator with spectral normalization, replacing the VGG-style global discriminator, giving per-pixel feedback and stabilizing GAN training under the more complex degradations
- Efficient on-the-fly synthesis of training pairs and a released, widely-adopted open-source implementation (incl. Real-ESRGAN and the lightweight Real-ESRNet/anime variants)

## Results
The paper reports superior perceptual/visual quality versus prior works (ESRGAN, BSRGAN, real-SR variants) on real-world test images, with notably fewer artifacts (reduced ringing/overshoot, better texture and noise handling); evaluation emphasizes qualitative and user-study/visual comparisons on diverse real datasets rather than reference PSNR/SSIM (no clean ground truth exists for real images). Trained primarily on DIV2K, Flickr2K, and OutdoorSceneTraining HR images for x4 SR.

## Significance
Real-ESRGAN became a de facto baseline and practical tool for real-world blind super-resolution, demonstrating that carefully engineered synthetic degradation plus a stronger discriminator can outperform prior real-SR methods (e.g., BSRGAN, ESRGAN) on real images without any real paired data. Its degradation pipeline and U-Net SN discriminator are now standard building blocks reused by many downstream real-SR and diffusion-based restoration works.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*