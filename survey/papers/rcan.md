# Image Super-Resolution Using Very Deep Residual Channel Attention Networks

- **Model / Abbrev:** RCAN
- **Venue:** ECCV 2018 (2018)
- **Category:** CNN Attention
- **Paper:** <https://arxiv.org/abs/1807.02758>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
CNN depth is crucial for SR accuracy, but very deep SR networks are hard to train. Moreover, LR inputs/features carry abundant low-frequency information that is treated equally across channels, wasting representational capacity and hindering the network's ability to focus on high-frequency detail.

## Method
RCAN builds a very deep (~400-layer) network using a Residual-in-Residual (RIR) structure: 10 residual groups, each with 20 residual blocks, stacked with long skip connections at the group level and short skip connections within blocks. These dense skip connections bypass low-frequency information so the trunk focuses on high-frequency learning, enabling stable training of a much deeper net than EDSR. Each residual block embeds a channel attention (CA) module that uses global average pooling to produce channel-wise statistics, then two FC layers (reduction ratio 16) with ReLU and a sigmoid gate to adaptively rescale per-channel features by their interdependencies. Trained end-to-end with L1 loss.

## Key Contributions
- Residual-in-Residual (RIR) architecture combining long and short skip connections to train very deep (~400-layer) SR networks and bypass low-frequency information
- First channel attention (CA) mechanism for SR: squeeze-and-excitation-style per-channel rescaling (global avg pool -> FC reduction r=16 -> ReLU -> FC -> sigmoid) to emphasize informative high-frequency channels
- Residual Channel Attention Block (RCAB) as the basic unit integrating CA into residual blocks
- State-of-the-art results across BI and BD degradation models and multiple scales (x2/x3/x4/x8) on five standard benchmarks

## Results
~16.6M params. Trained on DIV2K (800 images), Adam, LR 1e-4 halved every 200 epochs, batch 16, 48x48 patches, L1 loss. BI degradation x4 PSNR/SSIM: Set5 32.62/0.8942, Set14 28.87/0.7889, B100 27.77/0.7436, Urban100 26.63/0.8034, Manga109 31.22/0.8854. x2: Set5 37.74/0.9590, Set14 33.79/0.9218, B100 32.41/0.9010, Urban100 31.66/0.9255, Manga109 38.27/0.9723. Outperforms EDSR, SRResNet, RDN, MemNet while using a much deeper but parameter-comparable design; geometric self-ensemble (RCAN+) gives further gains.

## Significance
RCAN was the first to bring channel attention to super-resolution and showed that depth beyond EDSR is exploitable if low-frequency signal is routed around the trunk via RIR. It became a long-standing CNN SR baseline and the template for subsequent attention-based and second-order attention SR networks (e.g., SAN, RDN variants).

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*