# EDVR: Video Restoration with Enhanced Deformable Convolutional Networks

- **Model / Abbrev:** EDVR
- **Venue:** CVPRW 2019 (2019)
- **Category:** Video SR
- **Paper:** <https://arxiv.org/abs/1905.02716>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Multi-frame video restoration (super-resolution and deblurring) faces two core challenges: (1) aligning multiple neighboring frames under large and complex motions, and (2) effectively fusing frames that carry diverse motion blur and occlusion artifacts. Explicit optical-flow alignment is inaccurate under large motion/blur and error-prone, and naive temporal fusion treats all frames/pixels equally.

## Method
Sliding-window architecture (2N+1 input frames -> one restored center frame), not recurrent. Alignment is done implicitly at the FEATURE level using deformable convolutions (DCNv2) rather than optical flow. The Pyramid, Cascading and Deformable (PCD) module aligns each neighbor to the reference in a coarse-to-fine manner: a 3-level feature pyramid where offsets and aligned features are predicted at low resolution and propagated upward, followed by an additional cascading deformable conv that refines the final aligned feature. The Temporal and Spatial Attention (TSA) fusion module computes per-frame temporal attention as dot-product similarity between each neighbor's embedding and the reference embedding (pixel-wise weights), then applies a spatial attention (pyramid) mask, fusing via element-wise multiplication/addition before reconstruction. Reconstruction is a stack of residual blocks plus upsampling. An optional second EDVR stage (cascading two-stage refinement) further removes residual misalignment/artifacts.

## Key Contributions
- PCD alignment: pyramid + cascading deformable convolutions performing implicit feature-level alignment in a coarse-to-fine manner, handling large/complex motions without explicit optical flow
- TSA fusion: temporal attention (embedding-similarity per frame) plus spatial attention to weight informative frames/regions during fusion
- Sliding-window, flow-free design generalizing across SR and deblurring tasks
- Two-stage cascading refinement (EDVR stacked) to suppress residual artifacts
- Won all four tracks of the NTIRE 2019 video restoration & enhancement challenge

## Results
Evaluated on REDS4, Vimeo-90K-T, and Vid4 for 4x SR. EDVR set state-of-the-art over RCAN, VESPCN, DUF, TOFlow and RBPN, and won 1st place by a large margin in all NTIRE19 tracks. Headline numbers from the paper (large model, EDVR-L): REDS4 ~31.09 dB; Vimeo-90K-T ~37.61 dB; Vid4 ~27.35 dB. NOTE: exact dB values could not be re-verified from the PDF (numeric result tables were embedded as non-extractable image streams), so these figures are reported from prior knowledge and should be confirmed against Tables in the original paper before citation.

## Significance
Established feature-level deformable-convolution alignment as a robust alternative to explicit optical-flow warping for multi-frame restoration, becoming a foundational and widely-cited baseline/backbone for subsequent video SR work (e.g., BasicVSR family, EDVR-based deblurring). Its PCD + TSA design is a canonical example of the sliding-window paradigm in VSR.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*