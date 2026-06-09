# AlignVAR: Towards Globally Consistent Visual Autoregression for Image Super-Resolution

- **Model / Abbrev:** AlignVAR
- **Venue:** CVPR 2026 (2026)
- **Category:** Generative/Autoregressive
- **Paper:** <https://arxiv.org/abs/2603.00589>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Visual autoregressive (VAR) models applied to image super-resolution suffer from two failures that break global consistency: locality-biased attention that fragments spatial structure, and residual-only (next-scale) supervision that accumulates errors across scales. The paper aims to make scale-wise visual autoregression globally consistent for SR while keeping VAR's speed advantage over diffusion.

## Method
A scale-wise autoregressive transformer (24 blocks, initialized from pretrained VAR weights, ~1.06B params) generates HR tokens over 10 progressive-resolution scales. Two modules fix consistency. Spatial Consistency Autoregression (SCA) reweights intra-scale attention via a learnable adaptive mask: an MLP mask generator takes concatenated tokens plus structure-aware guidance (Laplacian edge cues extracted from the LR input) and applies element-wise gating r̃_k = (1+m_k) ⊙ r_k, steering attention toward structurally correlated regions and strengthening long-range dependencies. Hierarchical Consistency Constraint (HCC) augments the residual-only cross-entropy objective with full-scale latent reconstruction supervision at every scale, enforcing ||û_pred^k − u_gt^k||² so accumulated deviations are exposed and corrected early rather than propagating.

## Key Contributions
- Identifies and formalizes two root causes of global inconsistency in VAR-based SR: locality-biased intra-scale attention and error-accumulating residual-only inter-scale supervision
- Spatial Consistency Autoregression (SCA): structure-aware (Laplacian edge guidance from LR) learnable adaptive masking that gates/reweights fine-grained intra-scale attention toward structurally correlated regions
- Hierarchical Consistency Constraint (HCC): joint residual + full-scale latent supervision across all 10 scales for early error correction, recalibrating multi-level inter-scale dependencies
- Demonstrates non-iterative scale-wise VAR can match/beat diffusion SR at >10x speed and ~50% fewer params (1,056.5M vs VARSR 1,102.9M)

## Results
512x512 inference in 0.43s, >10x faster than diffusion SR (PASD 5.94s, StableSR 15.32s). DIV2K-Val: FID 25.71 vs VARSR 28.64, LPIPS 0.2955 vs 0.2985. RealSR: MUSIQ 68.53 vs 66.65, CLIPIQA 0.6784 vs 0.5953. DRealSR: MUSIQ 63.83 vs 62.66. Params 1,056.5M vs VARSR 1,102.9M.

## Significance
Shows that scale-wise visual autoregression, properly constrained for spatial and hierarchical consistency, is a viable high-fidelity alternative to diffusion for SR — delivering an order-of-magnitude faster, non-iterative inference with fewer parameters while improving both perceptual (FID/MUSIQ/CLIPIQA) and fidelity (LPIPS) metrics over the prior VAR-SR baseline (VARSR).

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*