# GSNR: Graph Smooth Null-Space Representation for Inverse Problems

- **Model / Abbrev:** GSNR
- **Venue:** CVPR 2026 (2026)
- **Category:** Other/Tangential
- **Paper:** <https://arxiv.org/abs/2602.20328>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Imaging inverse problems (deblurring, compressed sensing, demosaicing, super-resolution) are ill-posed: the sensing operator H has a non-trivial null-space, so the "invisible" component of the solution is unconstrained by measurements. Standard priors (sparsity, total-variation smoothness) act on the whole image and do not specifically regularize this null-space, biasing reconstructions and inviting hallucinations.

## Method
GSNR imposes graph-smoothness structure exclusively on the null-space component via the range-null decomposition x = x_r + x_n, with range projector P_r = H†H and null projector P_n = I − H†H. It builds a null-restricted graph Laplacian T = P_n L P_n (L a sparse PSD 4-/8-nearest-neighbor grid graph Laplacian), eigendecomposes it, and forms a low-dimensional projection matrix S = V[:, 1:p]^T from the p smoothest (lowest graph-frequency) eigenmodes. A small U-Net predictor G is trained to regress these coefficients from measurements (argmin_G E||G(y) − Sx*||²), and the resulting GSNR prediction penalty plus a graph regularizer are plugged into existing solvers (PnP, DIP, diffusion) alongside data fidelity and the learned prior. This restricts the invisible component to a smooth low-dimensional graph-spectral subspace.

## Key Contributions
- First method to endow specifically the null-space component with a graph structure, via a sparse PSD null-restricted Laplacian T = P_n L P_n, rather than smoothing the entire image
- Low-dimensional projection onto the p-smoothest null-space graph-Fourier modes, improving conditioning/convergence, null-space variance coverage, and predictability of modes from measurements
- Plug-in module compatible with PnP, Deep Image Prior (DIP), and diffusion solvers (e.g. DiffPIR) across four tasks
- Lightweight U-Net coefficient predictor (4-level enc/dec, 64 base channels, Adam lr 1e-3, ~50 epochs) trained to map measurements to null-space mode coefficients

## Results
Up to +4.3 dB PSNR over baseline solver formulations and up to +1 dB vs end-to-end learned models. CelebA SR (SRF=4): GSNR-PnP 29.42 dB vs PnP baseline 27.37 dB. Demosaicing (CelebA): 39.89 vs 39.35 dB. Deblurring (CelebA, Lip-DnCNN): 38.18 vs 37.86 dB (NPN). Diffusion SR (DiffPIR+GSNR): 30.48 dB (~+0.3 dB). Benchmarks: CIFAR-10, CelebA, Places365, plus SAR imagery; CS at m/n=0.1.

## Significance
Reframes prior design around the operator's null-space: by constraining only the unobservable component with a graph-spectral smoothness basis, it reduces hallucination/bias and improves conditioning, giving consistent gains as a drop-in across optimization-, DIP-, and diffusion-based inverse-problem solvers without retraining the base solver.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*