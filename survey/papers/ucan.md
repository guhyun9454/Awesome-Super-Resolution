# UCAN: Unified Convolutional Attention Network for Expansive Receptive Fields in Lightweight Super-Resolution

- **Model / Abbrev:** UCAN (and larger variant UCAN-L)
- **Venue:** CVPR 2026 (2026)
- **Category:** Efficient/Lightweight SR
- **Paper:** <https://arxiv.org/abs/2603.11680>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Hybrid CNN-Transformer SR networks get strong accuracy by enlarging attention windows or convolution kernels, but this scaling drives up computation/memory quadratically, making them impractical on resource-constrained devices. UCAN targets expanding the effective receptive field for lightweight SR without the usual cost blow-up.

## Method
UCAN unifies convolution and attention to grow the effective receptive field cheaply. The backbone is a chain of Broad Effective Receptive Field Groups (BERFGs); each BERFG pairs a Sharing Block (computes full attention maps) with a Receiving Block (reuses those maps) — a semi-parameter-sharing scheme. Each block stacks High-Performance Attention (32x32 windows via Flash Attention), a three-branch Hybrid Attention module (window MHSA for local spatial, linear Hedgehog Attention with depthwise conv for global spatial, softmax channel attention — all projecting C to C/2), a Large Kernel Distillation layer (splits channels into fine/coarse; fine channels go through triple-branch feature extraction with channel attention, a 1x1-3x3-1x1 local bottleneck, and hierarchical depthwise/dilated large-kernel convs), and Enhanced Spatial Attention (ESA). The Hedgehog Feature Map concatenates m symmetric exponential feature pairs to prevent rank collapse in linear attention while keeping linear complexity. Trained with L1 + LDL + wavelet losses.

## Key Contributions
- Hedgehog Attention for linear-complexity global modeling that avoids rank collapse via a Hedgehog Feature Map of symmetric exponential pairs, replacing quadratic softmax attention
- BERFG with semi-parameter sharing (Sharing Block computes attention maps, Receiving Block reuses them) to cut redundant attention computation while preserving cross-layer representation diversity
- Distillation-based Large Kernel module (LKD) that processes only fine-grained channels through parallel channel-attention/local-bottleneck/hierarchical-large-kernel branches to capture high-frequency detail and expand receptive field cheaply
- Three-branch Hybrid Attention unifying local window self-attention, global linear attention, and channel attention
- State-of-the-art lightweight SR accuracy-efficiency trade-off across standard benchmarks at ~0.7M params

## Results
~689K params at x2, ~705K at x4. x2: Set5 38.34/38.37 (UCAN/UCAN-L), Urban100 33.22/33.39, Manga109 39.54/39.66 dB. x4: Set5 32.65/32.68, BSD100 27.79/27.80, Urban100 26.89/27.06, Manga109 31.50/31.63 dB at 38.1G MACs (UCAN-L Manga109 at 48.4G). Outperforms recent lightweight models and methods with much larger footprints.

## Significance
Shows that linear (Hedgehog) attention plus large-kernel distillation and semi-parameter sharing can deliver expansive receptive fields and transformer-level accuracy in a sub-1M-parameter SR network, pushing the lightweight SR Pareto frontier for edge deployment. Ablations confirm 32x32 attention windows, mid-size kernels, and semi- (not full) sharing are each necessary.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*