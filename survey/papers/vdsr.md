# Accurate Image Super-Resolution Using Very Deep Convolutional Networks

- **Model / Abbrev:** VDSR
- **Venue:** CVPR 2016 (2016)
- **Category:** CNN (Deep Residual)
- **Paper:** <https://arxiv.org/abs/1511.04587>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Single-image super-resolution (SISR): reconstruct a high-resolution image from a low-resolution input. Prior CNN-based SR (SRCNN) used only 3 shallow layers with a small receptive field, limiting accuracy, training only at a single scale, and converging slowly.

## Method
A very deep (20 weight layer) CNN inspired by VGG-net, using cascaded 3x3 convolutions to build a large receptive field that exploits broad contextual information. The bicubic-upsampled LR image is fed in at HR resolution; the network learns only the residual (high-frequency detail) between input and ground-truth HR, then adds it back. Trained with very high learning rate (10^4x SRCNN's) made stable via adjustable gradient clipping (clip threshold scaled by the current learning rate), which dramatically speeds convergence. A single network is trained jointly on multiple scale factors (x2, x3, x4) and handles all of them.

## Key Contributions
- Demonstrated that increasing depth (20 layers vs SRCNN's 3) substantially improves SR accuracy via a larger contextual receptive field
- Residual learning: network predicts only the high-frequency residual, easing optimization and accelerating convergence for very deep SR nets
- Adjustable gradient clipping enabling training at learning rates ~10^4x higher than SRCNN without divergence/exploding gradients
- Single multi-scale model: one trained network handles x2/x3/x4 simultaneously, rather than a separate model per scale
- Zero-padding each layer to keep feature/output sizes equal to the input, avoiding the border-cropping/center-prediction issues of SRCNN

## Results
~37.53 dB PSNR on Set5 (x2), surpassing SRCNN by roughly 0.87 dB; consistent PSNR/SSIM gains over Bicubic, A+, RFL, SelfEx, and SRCNN across Set5, Set14, B100, and Urban100 at scales x2/x3/x4. Also faster at inference than SRCNN despite far greater depth.

## Significance
VDSR established that depth + residual learning is the path to high-accuracy CNN super-resolution, breaking SRCNN's shallow-network ceiling and introducing residual/global-skip learning and multi-scale training that became standard design patterns in nearly all subsequent SR networks.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*