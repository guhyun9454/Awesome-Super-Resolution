# SR3R: Rethinking Super-Resolution 3D Reconstruction With Feed-Forward Gaussian Splatting

- **Model / Abbrev:** SR3R
- **Venue:** CVPR 2026 (2026)
- **Category:** 3D/Gaussian SR
- **Paper:** <https://arxiv.org/abs/2602.24020>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
3D super-resolution (3DSR) reconstructs high-resolution 3D scenes from low-resolution multi-view images. Prior methods need dense LR inputs and per-scene optimization, and inherit high-frequency priors only from pretrained 2D SR models — limiting fidelity, cross-scene generalization, and real-time use.

## Method
SR3R reformulates 3DSR as a direct feed-forward mapping from sparse LR views to HR 3DGS, letting the network learn 3D-specific high-frequency geometry/appearance from large-scale multi-scene data rather than relying on a 2D-SR prior. It is a plug-and-play module on feed-forward 3DGS backbones (NoPoSplat, DepthSplat). The mapping net = ViT encoder, a bidirectional cross-attention feature-refinement module (aligns encoder tokens with geometry-aware tokens from the frozen 3DGS backbone to suppress upsampling ambiguity), a ViT decoder, and a Gaussian offset-learning head. Offset learning densifies each LR Gaussian into six sub-Gaussians along principal axes (scaffold G_Dense), then a PointTransformerV3 predicts residual corrections Δ(μ, α, r, s, c) giving G_HR = G_Dense + ΔG.

## Key Contributions
- Recasts 3D super-resolution as a feed-forward LR-views to HR-3DGS regression, eliminating per-scene optimization and dependence on 2D-SR priors
- Gaussian offset learning: densify each Gaussian into 6 sub-Gaussians as a scaffold and predict residual offsets via PointTransformerV3 — largest accuracy gain while cutting params ~3x vs naive upsampling (16.5M vs 44.5M)
- Bidirectional cross-attention feature refinement injecting backbone 3D geometric priors into 2D feature space to suppress upsampling-induced ambiguities
- Plug-and-play, backbone-agnostic design validated on both NoPoSplat and DepthSplat

## Results
4x 3DSR (64→256). RE10K: SR3R(DepthSplat) 26.25 PSNR / 0.856 SSIM / 0.165 LPIPS vs DepthSplat 23.15/0.699/0.281; SR3R(NoPoSplat) 24.79/0.827/0.188 vs 21.33/0.612/0.307. ACID: SR3R(DepthSplat) 27.02/0.797/0.261. Zero-shot RE10K→DTU: SR3R(NoPoSplat) 17.24/0.607/0.291 at 1.69s vs per-scene SRGS 12.42/300s, FSGS+SRGS 13.72/420s. ~14-16.5M Gaussians. Trained 75K iters, batch 8, lr 2.5e-5 on 4x RTX 5090; losses = MSE + LPIPS perceptual. Benchmarks: RE10K, ACID (train/test), DTU and ScanNet++ (zero-shot).

## Significance
Shows that learning 3D high-frequency detail end-to-end from multi-scene data beats lifting 2D-SR outputs, delivering sharper detail, more stable geometry, real-time feed-forward inference (~1.7s vs 300-420s for per-scene SRGS/FSGS), and strong zero-shot cross-dataset generalization for sparse-view 3D super-resolution.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*