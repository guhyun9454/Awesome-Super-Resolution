# Enhanced Deep Residual Networks for Single Image Super-Resolution

- **Model / Abbrev:** EDSR (and multi-scale variant MDSR)
- **Venue:** CVPRW 2017 (2017)
- **Category:** CNN Architecture
- **Paper:** <https://arxiv.org/abs/1707.02921>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Single-image super-resolution (SISR): reconstruct a high-resolution image from a low-resolution input. Prior CNN-based SR networks (e.g., SRResNet) inherited residual-block designs from high-level vision tasks (ResNet) that are suboptimal for SR, and most models were limited to a single fixed upscaling factor.

## Method
EDSR is a deep residual CNN that adapts the SRResNet architecture by removing Batch Normalization layers from the residual blocks, which the authors argue normalize away useful range flexibility and waste memory; the freed memory budget allows building a much larger/deeper network. Residual blocks use Conv-ReLU-Conv with a residual scaling factor (0.1) to stabilize training of wide networks. The body operates at LR resolution and upsampling is done at the end via sub-pixel (pixel-shuffle) convolution layers, with a global skip connection. MDSR is a single multi-scale model with scale-specific pre-processing and upsampling modules sharing a common deep body to handle x2/x3/x4 jointly.

_Trained with L1 (mean absolute error) loss rather than the conventional L2/MSE; ADAM optimizer; trained on DIV2K (800 training images) plus evaluated on standard benchmarks. Larger-scale models (x3/x4) initialized from the pretrained x2 model._

## Key Contributions
- Showed that removing Batch Normalization from residual blocks improves SR quality and cuts memory ~40%, enabling larger models
- Scaled up to a wide/deep network (EDSR: 32 residual blocks, 256 feature channels, ~43M params) using residual scaling (0.1) for stable training
- Introduced MDSR, a single multi-scale model handling x2/x3/x4 with shared body and scale-specific heads/tails
- Trained on the new high-quality DIV2K dataset; used L1 loss instead of L2 for better convergence/performance
- Won the NTIRE 2017 Single Image Super-Resolution Challenge (both tracks)
- Proposed geometric self-ensemble (x8) at test time for further gains

## Results
State-of-the-art PSNR/SSIM at the time on Set5, Set14, B100, Urban100 and DIV2K validation. Representative reported figures: Set5 x4 ~32.46 dB PSNR, Set14 x4 ~28.80 dB, B100 x4 ~27.71 dB, Urban100 x4 ~26.64 dB (with self-ensemble EDSR+ slightly higher), outperforming VDSR and SRResNet. Won NTIRE 2017 SR Challenge.

## Significance
EDSR established the design principle that BN is harmful for super-resolution and that simply scaling up width/depth of BN-free residual networks yields large gains, making it a foundational, long-standing baseline architecture (residual block design widely reused in RCAN, RDN, and many later SR/restoration models).

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*