# SwinIR: Image Restoration Using Swin Transformer

- **Model / Abbrev:** SwinIR
- **Venue:** ICCVW 2021 (2021)
- **Category:** Transformer SR
- **Paper:** <https://arxiv.org/abs/2108.10257>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
CNN-based image restoration suffers from content-independent convolution kernels and ineffective long-range dependency modeling, while early transformer restorers (e.g. IPT) are extremely heavyweight (115M+ params). SwinIR adapts the Swin Transformer's window-based attention to low-level vision to get strong restoration with far fewer parameters.

## Method
A three-stage architecture: (1) shallow feature extraction via a single 3x3 convolution; (2) deep feature extraction via a stack of Residual Swin Transformer Blocks (RSTBs); (3) high-quality reconstruction (sub-pixel/pixel-shuffle upsampling for SR, or a convolution for denoising/JPEG). Each RSTB contains several Swin Transformer Layers (window-based and shifted-window multi-head self-attention with MLPs) followed by a 3x3 conv and a residual connection, combining local window attention, cross-window interaction via shifting, and CNN-style inductive bias. A long residual connection links shallow to deep features so deep blocks focus on high-frequency recovery.

## Key Contributions
- Brings Swin Transformer's shifted-window self-attention to image restoration, unifying SR, denoising, and JPEG artifact reduction in one architecture
- Introduces the Residual Swin Transformer Block (RSTB): Swin Transformer layers + a conv layer + residual shortcut, merging attention-based long-range modeling with convolutional locality and stable training
- Demonstrates large parameter efficiency: matches or beats CNN and transformer SOTA with up to ~67% fewer parameters (11.8M vs IPT's 115M+)
- Provides classical, lightweight, and real-world SR variants plus color/grayscale denoising and JPEG compression artifact reduction models

## Results
Up to +0.45dB PSNR over prior SOTA on SR benchmarks (Set5/Set14/BSD100/Urban100/Manga109); up to +0.30dB on grayscale/color denoising (BSD68, Urban100, etc.); up to +0.16dB on JPEG artifact reduction (Classic5, LIVE1). Classical-SR model ~11.8M params vs IPT's >115M. Trained with L1 pixel loss (Charbonnier for some tasks), Adam optimizer, DIV2K/DF2K training, PSNR/SSIM evaluated on Y-channel.

## Significance
SwinIR established the Swin/window-attention transformer as the dominant backbone for low-level vision, showing transformers can beat CNNs (RCAN, EDSR) at a fraction of IPT's cost. It became a standard baseline and architectural template for nearly all subsequent transformer-based restoration work.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*