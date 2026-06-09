# ShiftLUT: Spatial Shift Enhanced Look-Up Tables for Efficient Image Restoration

- **Model / Abbrev:** ShiftLUT
- **Venue:** CVPR 2026 (2026)
- **Category:** Efficient/Lightweight SR
- **Paper:** <https://arxiv.org/abs/2603.00906>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Look-Up Table (LUT) based image restoration enables extremely fast, hardware-friendly inference on edge devices by replacing convolutions with precomputed table lookups, but suffers from a small, fixed receptive field (RF) because LUT indexing cost grows exponentially with the number of input pixels indexed. ShiftLUT targets expanding the effective receptive field of LUT-based restoration without sacrificing the storage and latency efficiency that makes LUTs attractive.

## Method
A LUT-based restoration framework built around three components. (1) A Learnable Spatial Shift (LSS) module applies learnable, channel-wise spatial offsets to feature maps, so that successive cheap LUT lookups aggregate information from spatially shifted positions, enlarging the effective receptive field without enlarging individual LUT index size. (2) An asymmetric / unbalanced dual-branch architecture allocates more compute to information-dense branches while keeping the other branch lightweight, optimizing the compute-quality trade-off and reducing latency. (3) An Error-bounded Adaptive Sampling (EAS) strategy compresses the LUTs at the feature level under a bounded error guarantee, minimizing storage overhead.

## Key Contributions
- Learnable Spatial Shift (LSS) module that uses channel-wise learnable spatial offsets to expand the receptive field of LUT lookups, reportedly achieving the largest receptive field among all LUT-based restoration methods
- Asymmetric/unbalanced dual-branch design that concentrates computation on information-dense branches to cut latency while preserving quality
- Error-bounded Adaptive Sampling (EAS) feature-level LUT compression that bounds reconstruction error while keeping storage small
- Demonstrates 3.8x larger receptive field than the prior SOTA TinyLUT with >0.21 dB average PSNR gain at comparable storage and inference time
- Public code release (GitHub)

## Results
Versus prior SOTA TinyLUT: 3.8x larger receptive field and over 0.21 dB average PSNR improvement across multiple standard benchmarks, while maintaining small storage size and inference time. The abstract does not enumerate per-dataset PSNR/SSIM, storage in KB/MB, or latency figures (those would be in the full PDF/repo).

## Significance
Pushes the efficiency frontier of LUT-based image restoration by overcoming the long-standing receptive-field bottleneck that limits LUT methods, making practical, near-instant edge-device super-resolution/restoration more accurate without trading away the storage and speed advantages over neural-network SR.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*